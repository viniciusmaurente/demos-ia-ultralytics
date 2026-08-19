# Demos ao vivo — Google Colab + Ultralytics

Cinco notebooks para as demonstrações da palestra 06, mais um de preparação.
Todos gerados por `tools/gerar_colabs.py` (edite o gerador, não o `.ipynb`).

## Links ativos (Google Drive do Vinicius) — atualizados em 19/08

Configurados em `EVENTO.demos` e testados: cada botão do deck abre o notebook
certo. Verificados por hash contra os arquivos locais — não foi confiado na
ordem em que os links chegaram.

| # | notebook | abrir no Colab |
|---|---|---|
| 00 | preparar o Drive | [1ZKQPTVIqj…](https://colab.research.google.com/drive/1ZKQPTVIqjknKrdVrUlu3n52P232tc6Zg) |
| 01 | detecção e contagem | [1whU_HIBtP…](https://colab.research.google.com/drive/1whU_HIBtPY6qYg9qbkJ4IfHqXrBT1831) |
| 02 | segmentação + instâncias ao vivo | [1gcQBDYdHG…](https://colab.research.google.com/drive/1gcQBDYdHG3ThnIznQjtFReQz6dYSACwc) |
| 03 | pose + polichinelo ao vivo | [1P3rOJCEGu…](https://colab.research.google.com/drive/1P3rOJCEGuQUS3PlP53ElCesgtwfPRpF7) |
| 04 | estoque + contador ao vivo | [1C149jsJMd…](https://colab.research.google.com/drive/1C149jsJMd7-B3OMIMPBf2PAmEvAb_aWn) |
| 05 | treino ao vivo (EPI) | [1csstfsrOd…](https://colab.research.google.com/drive/1csstfsrOd4FE18AhoyLW3xui3NoPd52-) |
| 06 | dataset → sistema rodando | [1aiCw15xWe…](https://colab.research.google.com/drive/1aiCw15xWeDM7giyKxagJ6TdSXH0Fnj06) |

Para trocar qualquer um depois (ou apontar para o GitHub):

```bash
python tools/configurar_demos.py --listar
python tools/configurar_demos.py --drive 01=<id>,03=<id>
python tools/configurar_demos.py --github usuario/repositorio
python tools/rebuild_inline_data.py 06     # SEMPRE depois
```

> **Se você editar um notebook no Colab e salvar**, o id não muda — o link
> continua valendo. Mas o arquivo do Drive passa a divergir da pasta
> `colabs/` deste projeto. A fonte de verdade aqui é `tools/gerar_colabs.py`.

| notebook | o que mostra no palco | precisa de treino? |
|---|---|---|
| `00-SETUP-DRIVE.ipynb` | **rodar em casa**, cria as pastas e baixa os modelos | — |
| `01-deteccao-contagem.ipynb` | a máquina vê, aponta e conta · o que é **confiança** | não |
| `02-segmentacao.ipynb` | caixa **vs** contorno · medir área ocupada | não |
| `03-pose-bracos.ipynb` | esqueleto, braços para cima, contagem de movimento | não |
| `04-estoque-garrafas.ipynb` | modelo pronto **+** especialista que você ensinou | sim, curto |
| `05-epi-treino-ao-vivo.ipynb` | **o treino acontecendo na tela**, época por época | sim, ao vivo |

---

## Ordem de uso

**Agora (em casa):** rode o `00`. Ele cria a árvore no seu Drive, baixa os quatro
modelos e imprime um checklist do que ainda falta de imagem.

**Depois:** preencha as pastas de imagem. O `00` tem uma célula de conferência —
rode de novo quantas vezes quiser, ela diz o que ainda falta.

**Na véspera:** rode `04` e `05` inteiros uma vez. Isso deixa os pesos treinados
salvos no Drive, então **no palco o treino é opcional**: se a GPU não vier, você
usa o modelo já pronto e mostra os gráficos do ensaio.

---

## As pastas no seu Drive

```
MyDrive/PALESTRA-IA/
├── 00-pesos/                     modelos base (o 00 baixa)
├── 01-deteccao/entrada|saida/
├── 02-segmentacao/entrada|saida/
├── 03-pose/entrada|saida/        vídeos entram aqui
├── 04-garrafas/
│   ├── inferencia/               fotos COM VÁRIAS garrafas (contar)
│   ├── treino/train|val/{lacrada,aberta}/
│   └── pesos/
├── 05-epi/
│   ├── treino/train|val/{com_epi,sem_epi}/
│   └── pesos/
└── 99-reserva/                   gravações de tela, plano B
```

### Quantas imagens

| pasta | mínimo | observação |
|---|---|---|
| `train/` de cada classe | 60–100 | quanto mais variado, melhor |
| `val/` de cada classe | 15–25 | **fotos que o treino nunca vê** |
| `04-garrafas/inferencia/` | 5–10 | cenas com várias garrafas juntas |

O `val/` não é burocracia: é a única prova de que o modelo **aprendeu** em vez de
ter decorado. Se você puser as mesmas fotos nos dois lugares, o número na tela
vai dar quase 100% e não vai significar nada.

---

## Pastas = gabarito

Para classificação, o YOLO lê o **nome da pasta como a resposta certa**. Você não
anota nada, não desenha caixa, não instala ferramenta: separa as fotos e pronto.

É a melhor frase do bloco de treino:
> *"eu não escrevi nenhuma regra sobre capacete. Eu separei fotos em duas pastas."*

**Detecção é diferente** — exige caixas marcadas imagem por imagem. Por isso o
`05` usa **classificação** (do seu material) para o treino ao vivo, que é onde
está o efeito, e não pede anotação nenhuma de você.

---

## Cuidados de palco

**Webcam no Colab é frame a frame.** O Colab roda num servidor remoto; a imagem
sobe do seu navegador quadro a quadro. Para a plateia, vídeo a 3 FPS lê como
"travou". A célula de webcam do `03` tira **uma foto** e analisa — o efeito de
ver a própria sala medida é imediato e não depende de fluidez. Vídeo fluido, só
rodando fora do Colab.

**GPU no Colab grátis não é garantida.** Com 20–30 épocas e imagens pequenas o
treino roda em CPU também, só mais devagar. Rode a célula de checagem antes de
subir ao palco — e tenha o resultado do ensaio no Drive.

**Grave a tela no ensaio.** Guarde em `99-reserva/`. Se a internet do local cair,
você mostra a gravação e conta exatamente a mesma história.

---

## Manutenção

```bash
python tools/gerar_colabs.py     # regera os .ipynb
python tools/validar_colabs.py   # JSON íntegro + todo o Python compila
```

O que foi testado de verdade antes de virar notebook, rodando ultralytics
8.3.156 localmente: detecção e `plot()`, pose com os 17 pontos, treino de
classificação com `results.csv`, e o callback por época que desenha a curva.

Duas armadilhas encontradas nesse teste e já tratadas no código:

1. **O callback dispara duas vezes na última época** (a validação final
   reaproveita o mesmo gancho). Se o histórico for uma lista, a curva ganha um
   ponto fantasma. Por isso é um dicionário indexado pela época.
2. **Keypoint que o modelo não enxerga volta como `(0,0)`** — e `y=0` é o topo da
   imagem, ou seja, um pulso escondido seria lido como *braço levantado*. Todo
   ponto passa por um piso de confiança antes de valer.
