# Demos ao vivo — Google Colab + Ultralytics

Cinco notebooks para as demonstrações da palestra 06, mais um de preparação.
Todos gerados por `tools/gerar_colabs.py` (edite o gerador, não o `.ipynb`).

## De onde os botões abrem: **GitHub**

Os cinco botões da palestra apontam para o repositório público:

**<https://github.com/viniciusmaurente/demos-ia-ultralytics>**

### Por que GitHub e não Drive

O link aponta para o arquivo **na fonte**, não para uma cópia. Quando o
notebook é atualizado aqui e vai para o GitHub, **o botão já abre a versão
nova** — sem você subir nada, sem id mudar, sem link quebrar.

Pela rota do Drive isso quebrou três vezes: cada vez que a pasta era apagada e
reenviada, os ids mudavam e os cinco botões morriam juntos.

**Os pesos treinados continuam indo para o seu Drive**, em qualquer uma das
rotas — quem salva são as próprias células (`shutil.copy` para
`/content/drive/...`), não o lugar de onde o notebook abriu.

### O preço: um clique a mais

Notebook aberto do GitHub faz o Colab avisar que ele *"não foi criado pelo
Google"* e pedir **"Executar mesmo assim"** na primeira execução. É um clique,
uma vez por sessão. **Faça isso no ensaio** para não descobrir no palco.

### Plano B — os mesmos notebooks no seu Drive

Se o GitHub falhar no dia, estes ids abrem as cópias do seu Drive:

```
00 setup        1EmdkYDB5a_p59lL3rq8ejXL6BJi3xEj3
01 deteccao     1Q-4322NzLuvuiOaUr8Ka9vTi38d7mmPE
02 segmentacao  1zkSGcdXup8gjs4FinugpFj2Xb-uvEaQZ
03 pose         1qetoB1xjAIiwt3Uu46H3IqWRD_jQWhDR
04 garrafas     1Ozg-aR2dJpaLM71RwWNFDFv7N7RVe5CV
05 epi/treino   10znGJy_a1lvRtlcUDpIXIVlJd7HesAZo
06 dataset      1G27O3MHeLcGRwv1Flob7i9bNUg8XmR-Z
```

Abra com `https://colab.research.google.com/drive/<id>`, ou troque os botões
de volta num comando:

```bash
python tools/configurar_demos.py --drive 01=1Q-4322NzLuvuiOaUr8Ka9vTi38d7mmPE,02=...
python tools/rebuild_inline_data.py 06
```

Para conferir para onde os botões apontam agora:
`python tools/configurar_demos.py --listar`

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

**Webcam ao vivo: funciona, e é fluida.** O `02`, o `03`, o `04` e o `06` têm
blocos de tempo real. O vídeo **não sai do navegador** — ele toca nativo, e só
um overlay PNG transparente (máscaras, caixas, contadores) volta do servidor.
Por isso parece fluido mesmo quando o processamento roda a 8–12 quadros por
segundo.

Cada bloco ao vivo tem um **botão verde de encerrar**. Use-o em vez de
interromper a célula: assim a câmera é liberada direito.

Se a câmera não abrir no computador do evento, o `03` também tem a célula de
**foto única** como plano B — menos impressionante, mas nunca falha.

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
