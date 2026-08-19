# Demos ao vivo — Visão Computacional com Ultralytics

Notebooks usados nas demonstrações ao vivo das palestras de IA de
**Vinícius Maurente** — Layer Intelligence.

São demos didáticas: a máquina detecta, conta, entende postura e **aprende
na frente da plateia**. Rodam no Google Colab, com GPU gratuita.

## Abrir no Colab

| demo | o que mostra | abrir |
|---|---|---|
| **00 · Preparar o Drive** | cria as pastas e baixa os modelos (rodar uma vez) | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/00-SETUP-DRIVE.ipynb) |
| **01 · A máquina que vê** | detecção, contagem e o que significa "confiança" | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/01-deteccao-contagem.ipynb) |
| **02 · Caixa ou contorno** | detecção × segmentação, e medir área ocupada | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/02-segmentacao.ipynb) |
| **03 · Postura ao vivo** | esqueleto, braços levantados, contagem de movimento | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/03-pose-bracos.ipynb) |
| **04 · Contando estoque** | modelo pronto **+** especialista treinado por você | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/04-estoque-garrafas.ipynb) |
| **05 · Ensinando ao vivo** | o treino acontecendo, época por época | [▶](https://colab.research.google.com/github/USUARIO/REPO/blob/main/colabs/05-epi-treino-ao-vivo.ipynb) |

> Trocar `USUARIO/REPO` pelos valores reais depois de publicar.

## Como usar

Comece pelo **00**: ele monta o seu Google Drive, cria a árvore de pastas e
baixa os modelos, para que nenhuma demo dependa de download na hora.

Depois preencha as pastas de imagem que ele indicar. O próprio notebook diz
quantas faltam em cada uma.

## A ideia por trás

Duas metades, e a segunda é a que vale:

| etapa | quem faz | precisou treinar? |
|---|---|---|
| achar pessoas, garrafas, objetos | modelo pronto (80 classes do COCO) | não |
| dizer se tem EPI, se a garrafa está aberta | modelo **que você ensinou** | sim |

O modelo genérico não sabe nada do seu negócio. A parte específica não vem
pronta — e ensiná-la é mais simples do que parece: **separar fotos em duas
pastas**. O nome da pasta é o gabarito.

## Detalhes que só aparecem rodando de verdade

Dois problemas encontrados em teste e já tratados no código:

1. **O callback de época dispara duas vezes na última** (a validação final
   reaproveita o mesmo gancho). Num histórico em lista, a curva de
   aprendizado ganha um ponto fantasma bem no fim. Por isso o histórico é
   um dicionário indexado pela época.
2. **Keypoint que o modelo não enxerga volta como `(0,0)`** — e `y=0` é o
   topo da imagem, ou seja, um pulso escondido seria lido como *braço
   levantado*. Todo ponto passa por um piso de confiança antes de valer.

## Webcam no Colab

O Colab roda num servidor remoto: a webcam é capturada no seu navegador e
sobe quadro a quadro, o que dá poucos FPS. Para demonstração, use a captura
de **uma foto** (é o que o notebook 03 faz) — o efeito é imediato e não
depende de fluidez. Vídeo fluido, só rodando localmente.

## Licença

MIT. Use, adapte e mostre para quem quiser.

Construído sobre [Ultralytics](https://github.com/ultralytics/ultralytics)
(AGPL-3.0) — a biblioteca não é redistribuída aqui; os notebooks a instalam
via `pip` na hora de rodar.
