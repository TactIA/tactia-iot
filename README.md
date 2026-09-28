# TactIA — Rede, falhas e requisitos

O TactIA é um smartwatch assistivo para pessoas com deficiência auditiva severa ou profunda. Ele escuta o ambiente, reconhece sons relevantes como buzinas, campainhas e sirenes, e avisa o usuário por vibração no pulso e por um ícone na tela. Toda a classificação roda dentro do próprio relógio, num ESP32-S3, sem Wi-Fi, sem servidor e sem aplicativo. O projeto faz parte do Projeto Integrador 2026.

Este repositório reúne a análise de arquitetura de rede do sistema, apresentada em slides. O arquivo principal é o PDF `TactIA___Rede__falhas_e_requisitos.pdf`, com 12 slides.

## O que a apresentação cobre

Mesmo sendo um sistema fechado, o relógio tem vários enlaces internos entre sensor, processador e atuadores, e cada um pode falhar de um jeito diferente. A apresentação percorre esse caminho do som até o alerta.

**Visão geral (slides 1 e 2).** O problema que o projeto resolve e a ideia central de um sistema fechado, dividido em percepção, inteligência e atuação.

**Seção 1, as redes (slides 3 e 4).** Os seis enlaces internos do relógio: microfones para o ESP32-S3 via I2S, ESP32-S3 para o driver háptico DRV2605L via I2C, driver para o motor de vibração, ESP32-S3 para a tela AMOLED via QSPI, touch para o ESP32-S3 via I2C com interrupção, e o chip de energia AXP2101 via I2C. Para cada um, a tabela mostra quem inicia a comunicação, a periodicidade, o volume de dados e de quem é a infraestrutura.

**Seção 2, mapa de falhas (slides 5 e 6).** Falhas silenciosas, como o NACK no I2C do atuador (o som é classificado, mas o pulso não vibra e ninguém é avisado) e a leitura de bateria travada. Também a falha parcial do display, em que a tela apaga mas o resto do sistema continua funcionando sem perceber.

**Seção 3, requisitos (slides 7 e 8).** Nove requisitos funcionais e seis não funcionais, todos com número e método de verificação, cobrindo latência, retenção na falha, disponibilidade, integridade, energia e privacidade.

**Seção 4, diagrama (slide 9).** O diagrama da rede, com os enlaces, o sentido de quem inicia cada conversa, onde o dado fica guardado em cada ponto e o ESP32-S3 marcado como ponto único de falha.

**Seção 5, o que o projeto não exige (slide 10).** Redundância, encapsulamento IP54 e retenção de dados de 15 dias, comparados com o que um sistema de escala maior, como o monitoramento de frota da SPTrans, precisa ter, e por que o TactIA pode abrir mão de cada um.

**Backup (slides 11 e 12).** O catálogo completo de falhas, com três modos de falha para cada um dos seis enlaces.

## Decisões que vale destacar

O relógio não depende de nenhuma rede externa, então privacidade e disponibilidade vêm do próprio desenho: nenhum áudio sai do dispositivo. O preço dessa escolha é que o ESP32-S3 concentra percepção, inteligência e atuação, e por isso é o ponto único de falha do sistema. O documento assume isso abertamente e registra também as lacunas conhecidas do protótipo, como a falta de checagem de ACK/NACK no driver háptico e de um autoteste do display.

## Arquivos

- `TactIA___Rede__falhas_e_requisitos.pdf`: a apresentação completa.
- `tactia_diagrama.drawio` e `tactia_diagrama.png`: o diagrama da rede em formato editável e exportado.

## Equipe

Ricardo, Guilherme, Diogo e Vinicius.
