# AI04 XiaoZhi PT-BR

Firmware nativo em português do Brasil para a placa `davinci-ai04-c3`, com
canal OTA próprio e instalador Windows conservador.

## Estado atual

- Baseline validado: XiaoZhi `2.5.3.4`, ESP32-C3 e flash de 8 MiB.
- A placa foi reconstruída para ES8311, OLED SSD1306, botão GPIO 9, seis LEDs
  WS2812 e monitor de bateria.
- O firmware usa o endpoint oficial do XiaoZhi somente para ativação e a
  configuração de backend. A oferta de firmware do AI04 vem exclusivamente do
  manifesto HTTPS próprio.
- Se esse manifesto não estiver disponível ou for inválido, a atualização é
  recusada. Assim uma imagem oficial em inglês não substitui o firmware PT-BR.

## Layout confirmado e migração segura

Um backup de fábrica de 8 MiB confirmou o layout real do DaVinci AI04 C3:
`model` em `0x10000`, `ota_0` em `0x100000` e `ota_1` em `0x480000`, com
3,5 MiB por slot. O instalador `2.5.3.4` valida esse layout antes de agir,
detecta qual slot oficial está ativo e escreve somente:

- a configuração Wi-Fi no NVS em `0x9000`; e
- o firmware PT-BR no slot OTA oficial que estava ativo.

Ele preserva bootloader, tabela de partições, `otadata`, a partição `model` e
o slot OTA inativo, que continua sendo a recuperação original. Nunca use o
instalador anterior baseado no mapa genérico de 8 MiB: ele não corresponde a
esta placa.

Antes de gravar um aparelho real, preserve um backup completo de 8 MiB de um
AI04 original. O backup pode conter credenciais e tokens: não o publique nem o
adicione a este repositório.

## OTA próprio

O firmware AI04 consulta o manifesto controlado em
`ai04-ota/manifest.json`; nele ficam somente a versão, a URL HTTPS do binário e
o checksum. O dispositivo não envia identidade, MAC, Wi-Fi ou configuração
para esse repositório. A ramificação de entrega OTA só deve ser criada depois
da validação em um aparelho e do backup original.

## Automação

O workflow semanal baixa o XiaoZhi oficial, aplica o overlay-base validado
`2.5.3.1` e o delta seguro
`AI04-2.5.3.1-to-2.5.3.4.patch`, valida o patch de NVS e o layout de fábrica,
e publica apenas um artefato candidato. Ele não altera o manifesto OTA nem
libera firmware para dispositivos. Isso permite acompanhar o upstream sem
transformar uma mudança não testada em atualização automática.

## Estrutura

- `AI04-XiaoZhi-Overlay-2.5.3.1.zip`: overlay-base validado da placa e do
  instalador.
- `AI04-2.5.3.1-to-2.5.3.4.patch`: delta reproduzível para layout de fábrica,
  instalador conservador e OTA nativo PT-BR.
- `.github/workflows/build-ai04.yml`: candidato semanal e reproduzível.

Não armazene senhas de Wi-Fi, tokens, chaves ou backups de aparelhos reais
neste repositório.
