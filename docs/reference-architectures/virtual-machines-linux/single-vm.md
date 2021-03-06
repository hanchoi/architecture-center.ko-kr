---
title: "Azure에서 Linux VM 실행"
description: "확장성, 복원력, 관리 효율성 및 보안에 주의하면서 Azure에서 Linux VM을 실행하는 방법입니다."
author: telmosampaio
ms.date: 12/12/2017
pnp.series.title: Linux VM workloads
pnp.series.next: multi-vm
pnp.series.prev: ./index
ms.openlocfilehash: 7caef46e53b42011b5a12ef53384c0352b9b9a72
ms.sourcegitcommit: c9e6d8edb069b8c513de748ce8114c879bad5f49
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2018
---
# <a name="run-a-linux-vm-on-azure"></a>Azure에서 Linux VM 실행

이 참조 아키텍처는 Azure에서 Linux VM(가상 머신)을 실행하는 데 대해 검증된 일련의 사례를 보여 줍니다. 여기에는 VM 프로비전과 함께 네트워킹 및 저장소 구성 요소에 관한 권장 사항이 포함됩니다. 이 아키텍처는 단일 VM 인스턴스를 실행하는 데 사용할 수 있으며 N 계층 응용 프로그램과 같은 복잡한 아키텍처의 기반이 됩니다. [**이 솔루션을 배포합니다.**](#deploy-the-solution)

![[0]][0]

*이 아키텍처 다이어그램이 포함된 [Visio 파일][visio-download]을 다운로드합니다.*

## <a name="architecture"></a>건축

Azure VM을 프로비전하려면 계산, 네트워킹 및 저장소 리소스와 같은 추가 구성 요소가 필요합니다.

* **리소스 그룹.** [리소스 그룹][resource-manager-overview]은 관련된 리소스를 보유하는 컨테이너입니다. 일반적으로 해당 수명과 리소스 관리자를 기준으로 솔루션의 리소스를 그룹화해야 합니다. 단일 VM 워크로드의 경우 모든 리소스에 대해 단일 리소스 그룹을 만들 수 있습니다.
* **VM**. 게시된 이미지 목록 또는 Azure Blob Storage에 업로드된 사용자 지정 관리되는 이미지나 VHD(가상 하드 디스크) 파일에서 VM을 프로비전할 수 있습니다. Azure에서는 CentOS, Debian, Red Hat Enterprise, Ubuntu 및 FreeBSD를 포함하여 인기 있는 다양한 Linux 배포판을 실행할 수 있습니다. 자세한 내용은 [Azure 및 Linux][azure-linux]를 참조하세요.
* **OS 디스크.** OS 디스크는 [Azure Storage][azure-storage]에 저장된 VHD이므로 호스트 컴퓨터가 중단되어도 계속 유지됩니다. Linux VM의 경우 OS 디스크는 `/dev/sda1`입니다.
* **임시 디스크.** VM은 임시 디스크를 사용하여 만들어집니다. 이 디스크는 호스트 컴퓨터의 실제 드라이브에 저장됩니다. Azure Storage에는 저장되지 **않으며** 다시 부팅되는 동안에 그리고 다른 VM의 수명 주기 이벤트 동안에 삭제될 수 있습니다. 페이지 또는 스왑 파일과 같은 임시 데이터에 대해서만 이 디스크를 사용합니다. Linux VM의 경우 임시 디스크는 `/dev/sdb1`이며 `/mnt/resource` 또는 `/mnt`에 탑재됩니다.
* **데이터 디스크.** [데이터 디스크][data-disk]는 응용 프로그램 데이터에 사용되는 영구 VHD입니다. 데이터 디스크는 OS 디스크와 같은 Azure Storage에 저장됩니다.
* **가상 네트워크(VNet) 및 서브넷.** 모든 Azure VM은 VNet에 배포되어 여러 서브넷으로 분할될 수 있습니다.
* **공용 IP 주소.** 공용 IP 주소는 예를 들어 SSH를 통해 VM과 통신하는 데 필요합니다.
* **Azure DNS**. [Azure DNS][azure-dns]는 Microsoft Azure 인프라를 사용하여 이름 확인을 제공하는 DNS 도메인에 대한 호스팅 서비스입니다. Azure에 도메인을 호스트하면 다른 Azure 서비스와 동일한 자격 증명, API, 도구 및 대금 청구를 사용하여 DNS 레코드를 관리할 수 있습니다.
* **NIC(네트워크 인터페이스)**. 할당된 NIC를 통해 VM을 가상 네트워크와 통신하도록 할 수 있습니다.
* **NSG(네트워크 보안 그룹)**. [네트워크 보안 그룹][nsg]은 네트워크 리소스에 대한 네트워크 트래픽을 허용하거나 거부하는 데 사용됩니다. NSG를 개별 NIC 또는 서브넷에 연결할 수 있습니다. 서브넷에 연결하는 경우 NSG 규칙이 해당 서브넷의 모든 VM에 적용됩니다.
* **진단** VM을 관리 및 문제 해결하는 데 진단 로깅이 중요합니다.

## <a name="recommendations"></a>권장 사항

이 아키텍처는 Azure에서 Linux VM을 실행하기 위한 기본 권장 사항을 보여 줍니다. 그러나 단일 실패 지점을 생성하므로 중요한 작업에 단일 VM을 사용하지 않는 것이 좋습니다. 더 높은 가용성을 위해 [가용성 집합][availability-set]에 여러 VM을 배포합니다. 자세한 내용은 [Azure에서 여러 VM 실행][multi-vm]을 참조하세요. 

### <a name="vm-recommendations"></a>VM 권장 사항

Azure는 다양한 가상 머신 크기를 제공합니다. [Premium Storage][premium-storage]는 높은 성능과 짧은 대기 시간으로 인해 권장되며 [특정 VM 크기에서 지원][premium-storage-supported]됩니다. 고성능 컴퓨팅과 같은 특수한 워크로드가 발생하지 않는다면 이러한 크기 중 하나를 선택합니다. 자세한 내용은 [가상 머신 크기][virtual-machine-sizes]를 참조하세요.

기존 워크로드를 Azure로 이동하는 경우 온-프레미스 서버와 가장 근접하게 일치하는 VM 크기부터 사용하기 시작합니다. 그런 다음 CPU, 메모리, 디스크 IOPS(초당 입력/출력 작업 수)에 따라 실제 워크로드의 성능을 측정하고 필요에 따라 크기를 조정합니다. VM에 여러 개의 NIC가 필요한 경우 각 [VM 크기][vm-size-tables]에 대한 최대 NIC 수가 정의되어 있습니다.

Azure 리소스를 프로비전할 때는 지역을 지정해야 합니다. 일반적으로 내부 사용자 또는 고객에게 가장 가까운 지역을 선택합니다. 그러나 일부 지역에서는 일부 VM 크기를 사용하지 못할 수 있습니다. 자세한 내용은 [지역별 서비스][services-by-region]를 참조하세요. 지정된 지역에서 사용할 수 있는 VM 크기 목록을 보려면 다음 Azure CLI(명령줄 인터페이스) 명령을 실행합니다.

```
az vm list-sizes --location <location>
```

게시된 VM 이미지를 선택하는 방법에 대한 자세한 내용은 [Linux VM 이미지 찾기][select-vm-image]를 참조하세요.

기본 상태 메트릭, 진단 인프라 로그 및 [부팅 진단][boot-diagnostics]을 모니터링 및 진단을 사용하도록 설정할 수 있습니다. 부팅 진단은 VM이 부팅할 수 없는 상태로 전환되는 경우 부팅 오류를 진단하는 데 도움이 될 수 있습니다. 자세한 내용은 [모니터링 및 진단 사용][enable-monitoring]을 참조하세요.  

### <a name="disk-and-storage-recommendations"></a>디스크 및 저장소 권장 사항

최상의 디스크 I/O 성능을 위해서는 SSD(반도체 드라이브)에 데이터를 저장하는 [Premium Storage][premium-storage]가 권장됩니다. 비용은 프로비전된 디스크 용량을 기준으로 산정됩니다. IOPS 및 처리량(즉, 데이터 전송 속도)도 디스크 크기에 따라 달라지므로 디스크를 프로비전할 때 세 가지 요소(용량, IOPS, 처리량)를 모두 고려합니다. 

또한 [관리 디스크](/azure/storage/storage-managed-disks-overview)를 사용하는 것이 좋습니다. 관리 디스크에는 저장소 계정이 필요하지 않습니다. 디스크의 크기와 유형을 지정하기만 하면 고가용성 리소스로 배포됩니다.

관리되지 않는 디스크를 사용 중인 경우 저장소 계정에 대한 [IOPS 제한][vm-disk-limits]에 도달하지 않도록 하기 위해 VHD(가상 하드 디스크)를 보유하는 각 VM에 대한 별도 Azure Storage 계정을 만듭니다.

하나 이상의 데이터 디스크를 추가합니다. VHD를 만들 때 형식은 지정되지 않습니다. 디스크를 포맷하려면 VM에 로그인합니다. 관리 디스크를 사용하고 있지 않고 데이터 디스크가 많은 경우 저장소 계정의 총 I/O 제한을 알아두어야 합니다. 자세한 내용은 [가상 머신 디스크 제한][vm-disk-limits]을 참조하세요.

Linux 셸에서 데이터 디스크는 `/dev/sdc`, `/dev/sdd` 등으로 표시됩니다. `lsblk` 를 실행하여 디스크를 포함하는 블록 장치를 나열할 수 있습니다. 데이터 디스크를 사용하려면 파티션 및 파일 시스템을 만들고 디스크를 탑재합니다. 예: 

```bat
# Create a partition.
sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.

# Create a file system.
sudo mkfs -t ext3 /dev/sdc1

# Mount the drive.
sudo mkdir /data1
sudo mount /dev/sdc1 /data1
```

데이터 디스크를 추가하면 디스크에 LUN(논리 단위 번호) ID가 할당됩니다. 예를 들어, 디스크를 교체하고 동일한 LUN ID를 유지하거나 특정 LUN ID를 검색하는 응용 프로그램이 있는 경우 필요에 따라 LUN ID &mdash;을(를) 지정할 수 있습니다. 그렇지만 LUN ID는 디스크마다 고유해야 합니다.

프리미엄 저장소 계정에서 VM의 디스크가 SSD이기 때문에 I/O 스케줄러를 변경하여 SSD의 성능을 최적화하려고 할 수 있습니다. SSD에 NOOP 스케줄러를 사용하는 것이 일반적으로 권장되지만 [iostat] 와 같은 도구를 사용하여 워크로드에 대한 디스크 I/O 성능을 모니터링할 수 있습니다.

성능을 최대화하기 위해 진단 로그를 저장할 별도의 저장소 계정을 만듭니다. 표준 LRS(로컬 중복 저장소) 계정은 진단 로그에 충분합니다.

### <a name="network-recommendations"></a>네트워크 권장 사항

이 공용 IP 주소는 동적 또는 정적일 수 있습니다. 기본값은 동적입니다.

* 변경되지 않는 고정 IP 주소가 필요한 경우 [정적 IP 주소][static-ip]를 예약합니다(예를 들어 DNS에 A 레코드를 만들어야 하는 경우 또는 안전한 목록에 추가될 IP 주소가 필요한 경우).
* IP 주소의 FQDN(정규화된 도메인 이름)을 만들 수도 있습니다. 그런 후 FQDN을 가리키는 DNS에 [CNAME 레코드][cname-record]를 등록할 수 있습니다. 자세한 내용은 [Azure Portal에서 정규화된 도메인 이름 만들기][fqdn]를 참조하세요. [Azure DNSS][azure-dns] 또는 다른 DNS 서비스를 사용할 수 있습니다.

모든 NSG에는 모든 인바운드 인터넷 트래픽을 차단하는 규칙을 포함하여 [기본 규칙][nsg-default-rules] 집합이 있습니다. 기본 규칙은 삭제할 수 없으나 다른 규칙으로 재정의할 수 있습니다. 인터넷 트래픽을 사용하도록 설정하려면 특정 포트(예: HTTP용 포트 80)로의 인바운드 트래픽을 허용하는 규칙을 만듭니다.

SSH를 사용하도록 설정하려면 TCP 포트 22에 인바운드 트래픽을 허용하는 NSG 규칙을 추가합니다.

## <a name="scalability-considerations"></a>확장성 고려 사항

[VM 크기를 변경][vm-resize]하여 VM을 규모 확장 또는 축소할 수 있습니다. 규모를 확장하려면 부하 분산 장치 뒤에 둘 이상의 VM을 배치합니다. 자세한 내용은 [Azure에서 확장성 및 가용성을 위해 여러 VM 실행][multi-vm]을 참조하세요.

## <a name="availability-considerations"></a>가용성 고려 사항

더 높은 가용성을 위해 가용성 집합에 여러 VM을 배포합니다. 그러면 [SLA(서비스 수준 계약)][vm-sla]도 높아집니다.

사용자의 VM은 [계획된 유지 관리][planned-maintenance] 또는 [계획되지 않은 유지 관리][manage-vm-availability]의 영향을 받을 수 있습니다. [VM 다시 부팅 로그][reboot-logs]를 사용하여 VM 재부팅이 계획된 유지 관리로 발생했는지 여부를 결정할 수 있습니다.

VHD는 [Azure Storage][azure-storage]에 저장됩니다. Azure Storage는 내구성 및 가용성을 위해 복제됩니다.

정상 작업 중 실수로 인한 데이터 손실(예: 사용자 오류 때문에 발생)을 막기 위해 [Blob 스냅숏][blob-snapshot] 또는 다른 도구를 사용하여 지정 시간 백업도 구현해야 합니다.

## <a name="manageability-considerations"></a>관리 효율성 고려 사항

**리소스 그룹** 동일한 수명 주기를 공유하는 긴밀하게 연결된 리소스를 동일한 [리소스 그룹][resource-manager-overview]에 배치합니다. 리소스 그룹을 사용하여 리소스를 그룹 단위로 배포 및 모니터링하고, 리소스 그룹별로 청구 비용을 추적할 수 있습니다. 리소스를 하나의 집합으로 삭제할 수도 있습니다. 이러한 기능은 테스트 배포에서 매우 유용합니다. 의미 있는 리소스 이름을 할당하면 간단하게 특정 리소스를 찾고 해당 역할을 이해할 수 있습니다. 자세한 내용은 [Azure 리소스에 대해 권장되는 명명 규칙][naming-conventions]을 참조하세요.

**SSH**. Linux VM을 만들기 전에 2048비트 RSA 공개-개인 키 쌍을 생성합니다. VM을 만들 때 공개 키 파일을 사용합니다. 자세한 내용은 [Azure에서 Linux 및 Mac과 함께 SSH를 사용하는 방법][ssh-linux]을 참조하세요.

**VM 중지.** Azure에서는 "중지됨"과 "할당 취소됨" 상태를 구분합니다. VM 상태가 중지되면 요금이 청구되지만 VM 할당이 취소되면 청구되지 않습니다.

Azure Portal에서 **중지** 버튼은 VM 할당을 취소합니다. 로그인한 상태에서 OS를 통해 종료하면 VM은 중지되지만 할당 취소되지 **않으므로** 비용이 계속 청구됩니다.

**VM 삭제.** VM을 삭제하는 경우 VHD는 삭제되지 않습니다. 즉, 데이터 손실 없이 안전하게 VM을 삭제할 수 있습니다. 그러나 저장소에 대한 비용은 계속 청구됩니다. VHD를 삭제하려면 [Blob 저장소][blob-storage]에서 파일을 삭제합니다.

실수로 삭제하지 않도록 하려면 [리소스 잠금][resource-lock]을 사용하여 전체 리소스 그룹을 잠그거나 VM과 같은 개별 리소스를 잠급니다.

## <a name="security-considerations"></a>보안 고려 사항

[Azure Security Center][security-center]를 사용하여 Azure 리소스의 보안 상태를 중앙에서 살펴볼 수 있습니다. Security Center는 잠재적인 보안 문제를 모니터링하고 배포의 보안 상태에 대한 종합적인 그림을 제공합니다. 보안 센터는 각 Azure 구독을 기준으로 구성됩니다. [Azure Security Center 빠른 시작 가이드][security-center-get-started]에 설명된 것처럼 보안 데이터 수집을 사용하도록 설정합니다. 데이터 수집이 사용되도록 설정되면 보안 센터는 해당 구독에서 만든 모든 VM을 자동으로 검색합니다.

**패치 관리.** 이 기능이 설정된 경우 Security Center는 보안 및 중요 업데이트 누락 여부를 확인합니다. 

**맬웨어 방지.** 이 기능이 설정되면 보안 센터는 맬웨어 방지 소프트웨어 설치 여부를 확인합니다. 또한 보안 센터를 사용하여 Azure 포털 내에서 맬웨어 방지 소프트웨어를 설치할 수도 있습니다.

**작업.** [RBAC(역할 기반 액세스 제어)][rbac]를 사용하여 배포하는 Azure 리소스에 대한 액세스를 제어합니다. RBAC를 통해 DevOps 팀의 구성원에게 권한 역할을 할당할 수 있습니다. 예를 들어 읽기 권한자 역할은 Azure 리소스를 볼 수 있지만 만들거나 관리하거나 삭제할 수는 없습니다. 일부 역할은 특정 Azure 리소스 유형에 따라 다릅니다. 예를 들어 Virtual Machine Contributor 역할은 VM을 다시 시작하거나 할당을 취소하고, 관리자 암호를 재설정하고, 새 VM을 만드는 등의 작업을 수행할 수 있습니다. 이 아키텍처에 유용할 수 있는 기타 [기본 제공 RBAC 역할][rbac-roles]에는 [DevTest Labs 사용자][rbac-devtest] 및 [네트워크 참가자][rbac-network]가 포함됩니다. 한 명의 사용자가 여러 역할에 할당될 수 있으며 좀 더 세분화된 권한의 사용자 지정 역할을 만들 수도 있습니다.

> [!NOTE]
> RBAC는 VM에 로그온한 사용자가 수행할 수 있는 작업을 제한하지 않습니다. 이러한 사용 권한은 게스트 OS의 계정 유형에 따라 결정됩니다.   

[감사 로그][audit-logs]를 사용하여 프로비전 동작 및 기타 VM 이벤트를 확인합니다.

**데이터 암호화.** OS 및 데이터 디스크를 암호화해야 하는 경우 [Azure Disk Encryption][disk-encryption]을 고려하세요. 

## <a name="deploy-the-solution"></a>솔루션 배포

이 아키텍처에 대한 배포는 [GitHub][github-folder]에서 사용할 수 있습니다. 다음을 배포합니다.

  * VM을 호스트하는 데 사용되는 **웹**이라는 이름의 단일 서브넷을 가진 가상 네트워크
  * SSH 및 HTTP 트래픽이 VM으로 이동할 수 있도록 허용하는 두 개의 들어오는 규칙이 있는 NSG
  * 최신 버전의 Ubuntu16.04.3 LTS를 실행하는 VM
  * 두 개의 데이터 디스크를 포맷하고 Apache HTTP Server를 Ubuntu VM에 배포하는 샘플 사용자 지정 스크립트 확장

### <a name="prerequisites"></a>필수 구성 요소

사용자의 구독에 참조 아키텍처를 배포하려면 먼저 다음 단계를 수행해야 합니다.

1. [AzureCAT 참조 아키텍처][ref-arch-repo] GitHub 리포지토리의 zip 파일을 복제, 포크 또는 다운로드합니다.

2. Azure CLI 2.0이 컴퓨터에 설치되어 있는지 확인합니다. CLI 설치 지침은 [Install Azure CLI 2.0][azure-cli-2](Azure CLI 2.0 설치)을 참조하세요.

3. [Azure 빌딩 블록][azbb] npm 패키지를 설치합니다.

4. 명령 프롬프트, bash 프롬프트 또는 PowerShell 프롬프트에서 다음 명령 중 하나를 사용하여 Azure 계정에 로그인한 다음 프롬프트에 따릅니다.

  ```bash
  az login
  ```

### <a name="deploy-the-solution-using-azbb"></a>azbb를 사용하여 솔루션 배포

샘플 단일 VM 워크로드를 배포하려면 다음 단계를 따릅니다.

1. 위의 필수 조건 단계에서 다운로드한 리포지토리의 `virtual-machines\single-vm\parameters\linux` 폴더로 이동합니다.

2. `single-vm-v2.json` 파일을 열고 아래 나와 있는 대로 큰따옴표 사이에 사용자 이름과 SSH 공개 키를 입력한 다음 파일을 저장합니다.

  ```bash
  "adminUsername": "",
  "sshPublicKey": "",
  ```

3. 아래 표시된 대로 `azbb`를 실행하여 샘플 VM을 배포합니다.

  ```bash
  azbb -s <subscription_id> -g <resource_group_name> -l <location> -p single-vm-v2.json --deploy
  ```

이 샘플 참조 아키텍처를 배포하는 방법에 대한 자세한 내용은 [GitHub 리포지토리][git]를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure 빌딩 블록][azbbv2]에 대해 알아봅니다.
- Azure에서 [여러 VM][multi-vm]을 배포합니다.

<!-- links -->
[audit-logs]: https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: /azure/virtual-machines/virtual-machines-linux-manage-availability
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[azbbv2]: https://github.com/mspnp/template-building-blocks
[azure-cli-2]: /cli/azure/install-azure-cli?view=azure-cli-latest
[azure-linux]: /azure/virtual-machines/virtual-machines-linux-azure-overview
[azure-storage]: /azure/storage/storage-introduction
[blob-snapshot]: /azure/storage/storage-blob-snapshots
[blob-storage]: /azure/storage/storage-introduction
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: /azure/virtual-machines/virtual-machines-linux-about-disks-vhds
[disk-encryption]: /azure/security/azure-security-disk-encryption
[enable-monitoring]: /azure/monitoring-and-diagnostics/insights-how-to-use-diagnostics
[azure-dns]: /azure/dns/dns-overview
[fqdn]: /azure/virtual-machines/virtual-machines-linux-portal-create-fqdn
[git]: https://github.com/mspnp/reference-architectures/tree/master/virtual-machines/single-vm
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/virtual-machines/single-vm
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: /azure/virtual-machines/virtual-machines-linux-manage-availability
[multi-vm]: multi-vm.md
[naming-conventions]: /azure/architecture/best-practices/naming-conventions.md
[nsg]: /azure/virtual-network/virtual-networks-nsg
[nsg-default-rules]: /azure/virtual-network/virtual-networks-nsg#default-rules
[planned-maintenance]: /azure/virtual-machines/virtual-machines-linux-planned-maintenance
[premium-storage]: /azure/virtual-machines/linux/premium-storage
[premium-storage-supported]: /azure/virtual-machines/linux/premium-storage#supported-vms
[rbac]: /azure/active-directory/role-based-access-control-what-is
[rbac-roles]: /azure/active-directory/role-based-access-built-in-roles
[rbac-devtest]: /azure/active-directory/role-based-access-built-in-roles#devtest-labs-user
[rbac-network]: /azure/active-directory/role-based-access-built-in-roles#network-contributor
[reboot-logs]: https://azure.microsoft.com/blog/viewing-vm-reboot-logs/
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[resource-lock]: /azure/resource-group-lock-resources
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[security-center]: /azure/security-center/security-center-intro
[security-center-get-started]: /azure/security-center/security-center-get-started
[select-vm-image]: /azure/virtual-machines/virtual-machines-linux-cli-ps-findimage
[services-by-region]: https://azure.microsoft.com/regions/#services
[ssh-linux]: /azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys
[static-ip]: /azure/virtual-network/virtual-networks-reserved-public-ip
[virtual-machine-sizes]: /azure/virtual-machines/virtual-machines-linux-sizes
[visio-download]: https://archcenter.azureedge.net/cdn/vm-reference-architectures.vsdx
[vm-disk-limits]: /azure/azure-subscription-service-limits#virtual-machine-disk-limits
[vm-resize]: /azure/virtual-machines/virtual-machines-linux-change-vm-size
[vm-size-tables]: /azure/virtual-machines/virtual-machines-linux-sizes
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[0]: ./images/single-vm-diagram.png "Azure의 단일 Linux VM 아키텍처"
