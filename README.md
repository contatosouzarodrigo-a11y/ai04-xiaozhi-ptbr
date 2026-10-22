# AI04 XiaoZhi PT-BR

Firmware nativo em português do Brasil para a placa `davinci-ai04-c3`, com canal OTA próprio e instalador Windows conservador.

## Estado atual

- Build validado: XiaoZhi `2.5.3.1`, ESP32-C3, flash de 8 MiB e dois slots OTA v2.
- A placa foi reconstruída para ES8311, OLED SSD1306, botão GPIO 9, seis LEDs WS2812 e monitor de bateria.
- O firmware não contém URL OTA fixa: o instalador configura o relay controlado no NVS.
- O relay mantém ativação e backend oficiais do XiaoZhi, mas substitui apenas a oferta de firmware do AI04. Se o manifesto próprio falhar, ele remove a oferta de atualização em vez de instalar uma imagem em inglês.

## Segurança de migração

A migração do layout legado v1 para v2 deve ser feita uma única vez por USB. Antes de gravar um aparelho real, é obrigatório preservar um backup completo de 8 MiB de um AI04 original e conferir o `partitions.bin` correspondente. O instalador lê os 8 MiB antes de gravar e altera somente as regiões necessárias.

## Automação

Este repositório usa o overlay validado contra o XiaoZhi oficial. O workflow semanal busca o upstream, tenta aplicar o overlay e gera um candidato somente quando a compilação e os testes passam. Publicação de OTA continua sendo uma etapa controlada: nenhuma atualização é liberada sem manifesto e binário compatíveis.

## Estrutura

- `ai04-overlay.zip`: overlay de código da placa, relay OTA e instalador.
- `.github/workflows/build-ai04.yml`: build reproduzível e candidato semanal.
- `tools/ai04-ota/`: relay Cloudflare Worker.
- `tools/ai04-installer/`: instalador USB Windows e patcher NVS.

Não armazene senhas de Wi-Fi, tokens ou chaves neste repositório.
