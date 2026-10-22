# AI04 XiaoZhi PT-BR

Firmware nativo em português do Brasil para a placa `davinci-ai04-c3`, com
canal OTA próprio e instalador Windows conservador.

## Estado atual

- Baseline validado: XiaoZhi `2.5.3.3`, ESP32-C3, flash de 8 MiB e dois slots
  OTA v2.
- A placa foi reconstruída para ES8311, OLED SSD1306, botão GPIO 9, seis LEDs
  WS2812 e monitor de bateria.
- O firmware usa o endpoint oficial do XiaoZhi somente para ativação e a
  configuração de backend. A oferta de firmware do AI04 vem exclusivamente do
  manifesto HTTPS próprio.
- Se esse manifesto não estiver disponível ou for inválido, a atualização é
  recusada. Assim uma imagem oficial em inglês não substitui o firmware PT-BR.

## Segurança de migração

A migração do layout legado v1 para v2 é feita uma única vez por USB. Antes de
gravar um aparelho real, preserve um backup completo de 8 MiB de um AI04
original e confira o `partitions.bin` correspondente. O instalador lê os 8 MiB
antes de gravar e altera somente as regiões necessárias.

## OTA próprio

O firmware AI04 consulta o manifesto controlado em
`ai04-ota/manifest.json`; nele ficam somente a versão, a URL HTTPS do binário e
o checksum. O dispositivo não envia identidade, MAC, Wi-Fi ou configuração
para esse repositório. A ramificação de entrega OTA só deve ser criada depois
da validação em um aparelho e do backup original.

## Automação

O workflow semanal baixa o XiaoZhi oficial, aplica o overlay-base validado
`2.5.3.1` e o delta nativo `AI04-2.5.3.1-to-2.5.3.3.patch`, executa os testes
e publica apenas um artefato candidato. Ele não altera o manifesto OTA nem
libera firmware para dispositivos. Isso permite acompanhar o upstream sem
transformar uma mudança não testada em atualização automática.

## Estrutura

- `AI04-XiaoZhi-Overlay-2.5.3.1.zip`: overlay-base validado da placa e do
  instalador.
- `AI04-2.5.3.1-to-2.5.3.3.patch`: delta reproduzível para o OTA nativo PT-BR.
- `.github/workflows/build-ai04.yml`: candidato semanal e reproduzível.

Não armazene senhas de Wi-Fi, tokens, chaves ou backups de aparelhos reais
neste repositório.
