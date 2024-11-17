# BASE EFI AMD - Ryzen and Threadripper (1XXX, 2XXX, 3XXX, 4XXX, 5XXX)

Nota|Descrição
:----|:----
Suporte inicial ao macOS|macOS 10.13, High Sierra.

- Versão do Opencore: 1.0.2
- Data de lançamento: 17/11/2024

# Passos Básicos

1. [Baixe](https://github.com/manuseiro/BASE-EFI-AMD-RYZEN-THREADRIPPER/releases) a última versão disponível;
2. Inclua kexts adicionais (para ethernet, áudio, etc);
3. Inclua os patches ACPI necessários (.aml);
4. Revise as notas especiais;
5. Gere e complete suas informações do SMBIOS;
6. Ajuste sua BIOS;
7. Instale o macOS e aproveite :)

# Funcionalidades
- [x] Muito leve, muito limpo, arquivos básicos para seu Hackintosh.
- [x] Feito com as versões mais recentes do OpenCore.
- [x] Duas edições - *Release* e *Debug* Edition.
- [x] Atualizado mensalmente com versões revisadas do OpenCore.

# Kexts incluídos (**Obrigatórios**)

Kext|Descrição
:----|:----
[AMDRyzenCPUPowerManagement.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|Para [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[SMCAMDProcessor.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)|Para [AMD Power Gadget](https://github.com/trulyspinach/SMCAMDProcessor).
[AppleMCEReporterDisabler.kext](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)|Útil a partir do Catalina para desativar o kext AppleMCEReporter que pode causar kernel panics em CPUs AMD e sistemas com dois processadores.
[Lilu.kext](https://github.com/acidanthera/Lilu/releases)|Corrige muitos processos, necessário para AppleALC, WhateverGreen, VirtualSMC e outros kexts.
[VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)|Emula o chip SMC encontrado em Macs reais, sem isso o macOS não inicializa.<br>Alternativa: FakeSMC, que pode ter suporte melhor ou pior, geralmente usado em hardwares antigos.
[WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)|Usado para correção gráfica, correções DRM, verificações de ID de placa, correções de framebuffer, etc; todas as GPUs se beneficiam deste kext.

# Outros Kexts (não inclusos)

Kexts para suporte de Áudio, Wifi, Ethernets e outros dispositivos.

### Audio

Kext|Descrição
:----|:----
255 / 5.000
[AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)|Usado para aplicação de patches no AppleHDA, permitindo suporte para a maioria dos controladores de som integrados.<br>O AMD 15h/16h pode ter problemas com isso e os sistemas Ryzen/Threadripper raramente têm suporte para microfone.
[VoodooHDA.kext](https://sourceforge.net/projects/voodoohda/)|Áudio para sistemas FX e suporte a microfone+áudio no painel frontal para sistemas Ryzen, não misture com AppleALC.<br>A qualidade do áudio é visivelmente pior que AppleALC em CPUs Zen.

### Ethernet
Kext|Descrição
:----|:----
[IntelMausi.kext](https://github.com/acidanthera/IntelMausi/releases)|Intel's 82578, 82579, I217, I218 and I219 NICs are officially supported.
[AtherosE2200Ethernet.kext](https://github.com/Mieze/AtherosE2200Ethernet/releases)|Required for Atheros and Killer NICs.<br>**Note**: Atheros Killer E2500 models are actually Realtek based, for these systems please use RealtekRTL8111 instead.
[RealtekRTL8111.kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)|For Realtek's Gigabit Ethernet.<br>Sometimes the latest version of the kext might not work properly with your Ethernet. If you see this issue, try older versions.
[LucyRTL8125Ethernet.kext](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/)|For Realtek's 2.5Gb Ethernet.
[SmallTreeIntel82576.kext](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases)| Required for I211 NICs, based off of the SmallTree kext but patched to support I211.<br>Required for most AMD boards running Intel NICs.

### WiFi and Bluetooth
Kext|Descrição
:----|:----
[AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)|Adds support for a large variety of Intel wireless cards and works natively in recovery thanks to IO80211Family integration.
[IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)|Adds Bluetooth support to macOS when paired with an Intel wireless card.
[AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)|Used for patching non-Apple/non-Fenvi Broadcom cards, will not work on Intel, Killer, Realtek, etc.<br>For Big Sur see [Big Sur Known Issues](https://dortania.github.io/OpenCore-Install-Guide/extras/big-sur#known-issues) for extra steps regarding AirPortBrcm4360 drivers.
[BrcmPatchRAM](https://github.com/acidanthera/BrcmPatchRAM/releases)|Used for uploading firmware on Broadcom Bluetooth chipset, required for all non-Apple/non-Fenvi Airport cards.

### Others
Kext|Descrição
:----|:----
[NVMeFix](https://github.com/acidanthera/NVMeFix/releases)|Used for fixing power management and initialization on non-Apple NVMe.
[SATA-Unsupported](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip)|Adds support for a large variety of SATA controllers, mainly relevant for laptops which have issues seeing the SATA drive in macOS.<br>We recommend testing without this first.
[AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)|Useful starting with Catalina to disable the AppleMCEReporter kext which will cause kernel panics on AMD CPUs.<br>Recommended for dual-socket systems (ie. Intel Xeons).

# Tabelas ACPI - AMD

Esses arquivos **DEVEM** ser incluídos no diretório ACPI do seu EFI. Recomendamos que você use o método **MANUAL**, mas para um primeiro teste você pode usar as versões prebuild.

Tabela|Descrição
:----|:----
SSDT-EC-USBX|[Manual](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-methods/manual.html) \| [Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml) \| [Details](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html)
SSDT-CPUR|[Prebuilt](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-CPUR.aml) <br> *Somente para B550 e A520*

### Despejando seu DSDT no ambiente Windows
[Download iASL Compiler ACPI Tools](https://acpica.org/downloads/binary-tools)
<br><br>
Open the CMD in the directory where the *ACPI Tools* was extracted. (*Command Prompt*) in **Administrator Mode**:
```
path/to/acpidump.exe -b -n DSDT -z
move dsdt.dat DSDT.aml
```

Decompile DSDT.aml:
```
path/to/iasl.exe path/to/DSDT.aml
```
*File DSDT.dsl will generated. Use this for generate YOUR ACPI Patches.* 

Compile DSDT.dsl:
```
path/to/iasl.exe path/to/DSDT.dsl
```
*File APCPI_FILE_PATCHED.aml will generated.*

# Attention

Update **config.plist** in PlatformInfo > Generic with YOUR informations.
<br><br>
*1. MLB (Board Serial)
<br>
2. ROM (Mac Address)
<br>
3. SystemSerialNumber (Serial)
<br>
4. SystemUUID (SmUUID)*

Please use [*genSMBIOS*](https://github.com/corpnewt/GenSMBIOS/archive/refs/heads/master.zip) for generate values for above itens.
<br>
Please use [*ProperTree*](https://github.com/corpnewt/ProperTree/archive/refs/heads/master.zip) for configure/edit your config.plist.

# SMBIOS compatível

SMBIOS|Descrição
:----|:----
iMacPro1,1|AMD RX Polaris e mais recentes.
MacPro7,1|AMD RX Polaris e mais recentes.<br>Observe que o MacPro7,1 é exclusivo para macOS 10.15, Catalina e mais recentes.
MacPro6,1|AMD R5/R7/R9 and older.
iMac14,2|Nvidia Kepler e mais recentes.<br>Observação: o iMac14,2 é suportado apenas para macOS 10.8-10.15, para macOS 11, Big Sur e mais recentes, use o MacPro7,1.

# Catalina e versões mais antigas do macOS

- Por favor, configure `MinDate` e `MinVersion` em UEFI > APFS para `-1`;
- Por favor, configure `SecureBootModel` em Misc > Security para `j137`;

\* *Sem as configurações acima, o macOS não conseguirá inicializar.*

# Special notes

- USB port mapping is **REQUIRED**.
- **`XhciPortLimit`** - Needed **`DISABLE`** if you use Big Sur 11.3+. 
	- Please Mapping USB in macOS Catalina before install Big Sur or Newer for best results.
	- You can use USBMap.command Utility - [USBMap](https://github.com/corpnewt/USBMap).
- **`SetupVirtualMap`** in config.plist:
	- B550, A520 and TRx40 boards should **`DISABLE`** this;
	- Newer BIOS versions of X570 also require this **`DISABLE`**;
	- X470 and B450 with late 2020 BIOS updates also require this **`DISABLE`**.
- **`DevirtualiseMmio`** - TRx40 users please **`ENABLE`** this flag.

## Special notes - Opencore 0.7.1+

Find the three `algrey - Force cpuid_cores_per_package` patches and alter the `Replace` value only - `config.plist`.

Changing `B8000000 0000`/`BA000000 0000`/`BA000000 0090`* to `B8 <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0090`* substituting `<CoreCount>` with the hexadeciamal value matching your physical core count.

**Note:** *The three different values reflect the patch for different versions of macOS. Be sure to change all three if you boot macOS 10.13 to macOS 12*

See the table below for the values matching your CPU Core Count.

| CoreCount | Hexadecimal|
|--------|---------|
|   6 Core  | `06` |
|   8 Core  | `08` |
|   12 Core | `0C` |
|   16 Core | `10` |
|   32 Core | `20` |

So for example a 6 Core 5600X Replace value would result in these replace values, `B8 06 0000 0000`/`BA 06 0000 0000`/`BA 06 0000 0090`

**Note:** *MacOS Monterey installation requires `Misc -> Security -> SecureBootModel` to be disabled in the config.<br />Also TPM needs to be disabled in the BIOS. Both can be enabled after install.*

## TRX40 Systems

Disabling the `mtrr_update_action - fix PAT` patch has shown an improvement in GPU performance on some systems that have tested. If you wish to test this it is recommended to do so on a USB with OpenCore to ensure it works first. There may be issues with different motherboard/GPU combos that we aren't aware of. Proceed at your own risk.

### General Purpose `boot-args`

Parameter|Description
:----|:----
npci=0x2000|This disables some PCI debugging related to `kIOPCIConfiguratorPFM64`, alternative is `npci=0x3000` which disables debugging related to `gIOPCITunnelledKey` in addition.<br>Required for when getting stuck on `PCI Start Configuration` as there are IRQ conflicts relating to your PCI lanes. **Not needed if Above4GDecoding is enabled.**
npci=0x3000|Alternative for `npci=0x2000`.

### GPU-Specific `boot-args`

Parameter|Description
:----|:----
agdpmod=pikera|Used for disabling board ID checks on Navi GPUs(RX 5000 series), without this you'll get a black screen.<br>**Don't use if you don't have Navi** (ie. Polaris and Vega cards shouldn't use this).
nvda_drv_vrl=1|Used for enabling Nvidia's Web Drivers on Maxwell and Pascal cards in Sierra and High Sierra.

# BIOS Settings

### Disable
- Fast Boot
- Secure Boot
- Serial/COM Port
- Parallel Port
- Compatibility Support Module (CSM)(Must be off, GPU errors like `gIO` are common when this option in enabled)

***Special note for 3990X users***: *macOS currently does not support more than 64 threads in the kernel, and so will kernel panic if it sees more. The 3990X CPU has 128 threads total and so requires half of that disabled. We recommend disabling hyper threading in the BIOS for these situations.*

### Enable
- Above 4G decoding
	- This must be on, if you can't find the option then add `npci=0x2000` to `boot-args`. 
	- Do not have both this option and `npci` in `boot-args` enabled at the same time.
	- If you are on a Gigabyte/Aorus or an AsRock motherboard, enabling this option may break certain drivers(ie. Ethernet) and/or boot failures on other OSes, if it does happen then disable this option and opt for `npci` instead.
	- 2020+ BIOS Notes: When enabling Above4G, Resizable BAR Support may become an available on some X570 and newer motherboards. Please ensure this is **`DISABLED`** instead of set to Auto.
- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode
- SATA Mode: AHCI

# References
https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html
<br>
https://dortania.github.io/Getting-Started-With-ACPI/
