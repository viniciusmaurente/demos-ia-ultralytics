# Como atualizar os notebooks no seu Drive

## ⚠ A armadilha: apagar e subir de novo **quebra os botões da palestra**

Os botões do deck apontam para o **id** de cada arquivo no Drive
(`colab.research.google.com/drive/<id>`). Esse id nasce com o arquivo.

Se você **apagar e subir de novo**, o arquivo ganha um id novo — e os cinco
botões da palestra passam a apontar para o nada.

**Faça sempre pelo "Gerenciar versões".** O arquivo continua o mesmo, o id não
muda, e os links seguem funcionando.

---

## O jeito certo, arquivo por arquivo

1. No Drive, **clique com o botão direito** no notebook (ex.: `03-pose-bracos.ipynb`)
2. **Gerenciar versões** → **Fazer upload de nova versão**
3. Escolha o arquivo novo em
   `palestra-IA-na-Pratica/colabs/03-pose-bracos.ipynb`
4. Pronto — mesmo id, conteúdo novo

> O Drive guarda as versões antigas por 30 dias. Se algo der errado no ensaio,
> dá para voltar pela mesma tela.

---

## Quais mudaram nesta rodada

| arquivo | o que mudou |
|---|---|
| `02-segmentacao.ipynb` | **novo bloco ao vivo**: instâncias de pessoa com id e máscara |
| `03-pose-bracos.ipynb` | **novo bloco ao vivo**: contador de polichinelo por pessoa |
| `04-estoque-garrafas.ipynb` | **novo bloco ao vivo**: contador de estoque em tempo real |
| `06-dataset-garrafas.ipynb` | **reescrito**: agora vai de dataset a sistema rodando |
| `00-SETUP-DRIVE.ipynb` | duas pastas novas (`dataset-yolo`, `recortes-para-separar`) |

O `01` e o `05` não mudaram.

### O que entrou no 06 nesta rodada

Ele deixou de parar no treino. Agora tem, na sequência: **inferência com os
pesos que você treinou** (em lote e comparada com o modelo pronto), **webcam ao
vivo com o seu detector**, **rotulagem assistida por CLIP** (a máquina dá o
primeiro palpite em todos os recortes e você só corrige os errados),
**treino do classificador** e o **pipeline completo** — detecta, classifica e
conta o estoque, em foto e ao vivo.

Nada mais de arrastar arquivo no Drive para separar aberta de lacrada.

### O 06 é arquivo novo

Esse pode subir normalmente (arrastar para o Drive), porque ainda não existe id
nenhum apontando para ele. Ele não tem botão na palestra — é notebook de
preparação, para rodar em casa.

---

## Alternativa: parar de mexer no Drive

Os mesmos notebooks estão em
**<https://github.com/viniciusmaurente/demos-ia-ultralytics>**, e eu atualizo
lá com um comando. Se você preferir que os botões apontem para o GitHub, é uma
linha:

```bash
python tools/configurar_demos.py --github viniciusmaurente/demos-ia-ultralytics
python tools/rebuild_inline_data.py 06
```

**O trade-off:** um notebook aberto do GitHub entra em modo somente leitura no
Colab — roda normalmente, mas pede "salvar uma cópia" para editar, e **não
guarda os pesos que você treinou**. Por isso a recomendação continua sendo:

- **botões da palestra → Drive** (é a sua cópia, com os pesos do ensaio)
- **GitHub → link público** para a plateia, e espelho se o Drive falhar

---

## Depois de atualizar, teste uma vez

Abra o `03` pelo botão da palestra (ou pela tecla `D` no slide 11) e rode até
a célula da webcam. Se a câmera abrir e o esqueleto aparecer, está tudo certo
para o palco.

**Ordem de teste recomendada, na véspera:**

1. `00` — confere que as pastas e os pesos estão no lugar
2. `05` — roda o treino inteiro uma vez, para os pesos ficarem salvos
3. `03` — webcam ao vivo (é a demo com mais peças móveis)
4. `04` — estoque, com uma garrafa na mão

E grave a tela enquanto testa: esse vídeo é o seu plano B se a internet do
local cair.
