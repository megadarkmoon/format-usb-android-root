# Como formatar um dispositivo de armazenamento USB no Termux com acesso root

## Antes de prosseguir!!!

- **EU NÃO SOU RESPONSÁVEL POR QUALQUER DANO QUE VOCÊ POSSA CAUSAR SE SEGUIR ESTES PASSOS INCORRETAMENTE**!!!
- Certifique-se de ler este guia pelo menos uma vez antes de prosseguir. Você foi avisado.
- Certifique-se de ter o [Magisk](https://github.com/topjohnwu/Magisk), [Apatch](https://github.com/bmax121/APatch) ou [KernelSU](https://github.com/tiann/KernelSU) instalado no seu dispositivo.
- Certifique-se de ter o sudo ou o tsu instalado, executando um dos seguintes comandos:

```bash
pkg update && pkg upgrade -y && pkg install sudo
```
 * Ou:
```bash
pkg update && pkg upgrade -y && pkg install tsu
```
 * **ATENÇÃO**: você não pode ter o sudo e o tsu instalados ao mesmo tempo. Se você já tiver o sudo instalado, a instalação do tsu falhará, e vice-versa.
## Atualizar e instalar binários
 * Atualize os binários do Termux e instale o blk-utils e o dosfstools:
 * **NOTA**: o dosfstools é necessário para formatar unidades como FAT32. Se não precisar dele, não instale, mas é bom tê-lo à mão. O Android padrão já deve vir com (mkfs.erofs, mkfs.exfat, mkfs.ext2, mkfs.ext3, mkfs.ext4, mkfs.ntfs).
```bash
pkg update && pkg upgrade -y && pkg install blk-utils dosfstools -y
```
## Encontrar e identificar o dispositivo de armazenamento USB
 * Encontre o caminho do dispositivo de armazenamento USB usando o lsblk do blk-utils:
```bash
sudo lsblk
```
 * Exemplo de saída:
```bash
sdf       8:80   1  3.7G  0 disk
└─sdf1    8:81   1  3.7G  0 part /mnt/media_rw/0124-1790 <-- Anote isso para mais tarde.
```
 * No meu caso, é sdf1.
## Identificar o ID do dispositivo de armazenamento USB e desmontar
 * Identifique o ID do dispositivo de armazenamento USB com o sm e desmonte-o:
```bash
sudo sm list-volumes
```
 * Exemplo de saída:
```bash
public:8,81 mounted 0124-1790 <-- O mesmo do lsblk.
private mounted null
emulated;0 mounted null
```
 * No meu caso, o ID é public:8,81.
```
sudo sm unmount public:8,81
```
 * Depois disso, a unidade será desmontada e poderemos prosseguir com a formatação.
## Formatar e remontar o dispositivo de armazenamento USB
 * Formate o dispositivo de armazenamento USB como preferir, usando qualquer um destes: (mkfs.erofs, mkfs.exfat, mkfs.ext2, mkfs.ext3, mkfs.ext4, mkfs.ntfs).
 * Eu estarei usando o mkfs.vfat do dosfstools.
```bash
sudo mkfs.vfat -F 32 -n MEUDISCO /dev/block/sdf1 <-- Altere isto!!!
```
 * **NOTA**: altere o caminho do bloco do seu dispositivo de armazenamento USB para o correspondente ao seu. No meu caso, é /dev/block/sdf1. Além disso, os blocos de dispositivos de armazenamento no Android ficam listados em /dev/block, e não em /dev.
 * Exemplo de saída:
```bash
mkfs.fat 4.2 (2021-01-31)
```
 * Agora podemos montar o dispositivo de armazenamento USB com o sm.
```bash
sudo sm mount public:8,81
```
 * Ou podemos montá-lo como armazenamento interno com:
```bash
sudo sm partition public:8,81 private
```
 * Não recomendado para dispositivos de armazenamento USB externos.
 * Eba! Você concluiu o guia com sucesso!
