# Banjo-Kazooie PT-BR — Lighthouse no R36S

Port **Lighthouse** (HarbourMasters) de **Banjo-Kazooie** para handhelds ARM
(R36S / dArkOS / ArkOS) com **tradução PT-BR** aplicada como *language pack*.

> ⚠️ **Sem ROM e sem assets de jogo.** O `bk.o2r` (assets extraídos da ROM) não é
> versionado — o próprio port o regenera a partir da **sua** ROM. Este repositório
> contém o binário do port, configurações, bibliotecas e o **language pack PT-BR**.

---

## 📦 Conteúdo

```
port/
├── Lighthouse.sh                 # launcher PortMaster
└── lighthouse/
    ├── Lighthouse                # binário aarch64 (build Jeod / PortMaster)
    ├── lighthouse.o2r            # assets do PORT (não do jogo)
    ├── port.json · config.yml · gameinfo.xml
    ├── lighthouse.gptk · gamecontrollerdb.txt
    ├── assets/yaml/{us,pal,jp}/  # definições p/ o Torch
    ├── libs/ · licenses/ · baseroms/
    └── mods/~lang/bk_pt-BR.o2r   # ⭐ language pack PT-BR (284 KB)
docs/
├── INSTALACAO_R36S.md            # passo a passo + ROMs compatíveis + troubleshooting
└── LANGPACK_TECNICO.md           # formatos, charset, como regenerar
scripts/
└── gerar_langpack_ptbr_lighthouse.py
evidencias/
└── evidence_langpack_ptbr_lighthouse_20261017.md
```

---

## 🎮 ROMs compatíveis

Validação por **SHA-1** (só estas 4 revisões geram o `bk.o2r`):

| Região / Revisão | SHA-1 | Nota |
|---|---|---|
| **US v1.0 (Rev 0)** | `1fe1632098865f639e22c11b9a81ee8f29c75d7a` | única base p/ romhacks |
| **US v1.1 (Rev 1)** | `ded6ee166e740ad1bc810fd678a84b48e245ab80` | **base deste port** |
| **PAL** | `bb359a75941df74bf7290212c89fbc6e2c5601fe` | origem da tradução BR |
| **JP** | `90726d7e7cd5bf6cdfd38f45c9acbf4d45bd9fd8` | adiciona japonês/kana |

Formato `.z64` (big-endian, magic `80 37 12 40`). ROM traduzida **não** é aceita
na extração — a tradução entra pelo language pack.

---

## 🚀 Instalação no R36S

1. Copie para o cartão:
   ```
   /roms2/ports/Lighthouse.sh
   /roms2/ports/lighthouse/...
   ```
   (algumas firmwares usam `/roms/ports`)
2. Coloque sua ROM em:
   ```
   /roms2/ports/lighthouse/baseroms/Banjo-Kazooie (U) (V1.1).z64
   ```
3. Inicie por **Ports**. No 1º boot o Torch embutido gera `bk.o2r` (~30 s).
4. Ative a tradução:
   ```
   Settings → General → Languages → Portuguese (BR)
   ```

Detalhes, controles e troubleshooting: [`docs/INSTALACAO_R36S.md`](docs/INSTALACAO_R36S.md).

---

## 🇧🇷 O que a tradução cobre

| | |
|---|---|
| 748 diálogos | todos os balões de fala |
| 169 perguntas | quiz da Furnace Fun |
| 29 perguntas | desafios da Grunty |
| Fonte `0x6EB` | 76 chunks com os acentos PT-BR |
| 16 strings de UI | tela de seleção de save + menu de pausa |

Tela de save traduzida: `JOGO 1 / : VAZIO / : TEMPO / QUEBRA-CABECA / NOTA`.

Detalhes técnicos: [`docs/LANGPACK_TECNICO.md`](docs/LANGPACK_TECNICO.md).

---

## 🔁 Regenerar o language pack

```bash
python3 scripts/gerar_langpack_ptbr_lighthouse.py
```

Requer Python 3, a ROM traduzida (`.z64`) e o `bk.o2r` base. O método genérico
(aplicável a outros ports) está em
[`n64-ports-ptbr`](https://github.com/wesleibborges/n64-ports-ptbr).

---

## ⚖️ Créditos e licenças

- **Lighthouse** — HarbourMasters (Malkierian, JeodC, Caladius, aMannus) — CC0-1.0
- **Torch** — HarbourMasters — MIT · **libultraship** — MIT
- **Build PortMaster (aarch64)** — Jeod
- **Tradução PT-BR** — Brazilian Warriors / Nintendo BR (Xujozer, Kamppello, Sergio-F)
- **Conversão para language pack** — este repositório

Nenhum asset de jogo é distribuído. Você precisa da sua própria ROM.
