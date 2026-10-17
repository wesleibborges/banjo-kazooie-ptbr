# Language Pack PT-BR — detalhes técnicos (Banjo-Kazooie / Lighthouse)

Documento específico do **Banjo-Kazooie**. O método genérico (aplicável a outros ports)
está no repositório [`n64-ports-ptbr`](https://github.com/wesleibborges/n64-ports-ptbr).

---

## 1. O que é entregue

`port/lighthouse/mods/~lang/bk_pt-BR.o2r` — 284.577 bytes, 1026 entradas.

| Categoria | Qtd | Formato |
|---|---|---|
| Diálogos | 748 | `LDKB` |
| Quiz (Furnace Fun) | 169 | `QQKB` |
| Perguntas da Grunty | 29 | `QGKB` |
| Fonte de diálogo `0x6EB` | 76 chunks | `PSKB` + `XETO` |
| Controles | 4 | `version`, `langinfo`, `portVersion`, `assets/portVersion` |

Metadados de idioma:

```
version  = 01 B1 CC 3F 73   -> 0xB1CC3F73 = BK_VER_PAL
langinfo = version 1 | 1 idioma | idx 0 | script 0 (Latin) | "Portuguese (BR)"
           + 16 strings de UI (tela de save + menu de pausa)
```

---

## 2. Origem do texto

Tradução brasileira de **Banjo-Kazooie** (Brazilian Warriors / Nintendo BR —
Xujozer, Kamppello, Sergio-F), feita sobre a ROM **PAL** (`NBKP`) com a ferramenta
de PacoChan. O pack reaproveita os bytes do texto **verbatim** da ROM traduzida.

Por que não usar a ROM traduzida direto?
- O Lighthouse valida SHA-1 da ROM para gerar o `bk.o2r` → ROM modificada é rejeitada.
- Romhack exige base US v1.0; a tradução é PAL → incompatível.
- Solução: manter o `bk.o2r` base (US v1.1) e sobrepor os assets via language pack.

---

## 3. Charset PT-BR

O texto é armazenado com os índices de caractere do jogo (não ASCII puro). Mapa
derivado empiricamente do próprio texto traduzido:

| Byte | Renderiza |
|---|---|
| `a` | Ç |
| `b` | É |
| `c` | Á |
| `d` | Ê |
| `g` | Í |
| `^` | Ó |
| `[` | Ã |
| `]` | Ü |
| `\` | Ö |

Os acentos só aparecem porque a fonte `0x6EB` do pack é a versão PAL/BR (72 dos 76
chunks diferem da US). A fonte bold `0x6EC` **não** foi alterada pela tradução —
por isso as 16 strings de UI usam texto sem acento (QUEBRA-CABECA, NAO, etc.).

---

## 4. Regenerar o pack

Pré-requisitos: Python 3, a ROM traduzida (`.z64`) e o `bk.o2r` base do port
(para copiar os headers de 64 bytes).

```bash
python3 scripts/gerar_langpack_ptbr_lighthouse.py
```

O script:
1. lê a tabela de assets em `0x5E90`;
2. descomprime cada asset BKZIP (`11 72` + zlib raw);
3. converte diálogo/quiz/grunty do formato N64 para O2R;
4. grava `version`, `langinfo` (com as 16 strings) e os assets.

A **fonte** (`0x6EB`) é extraída com o Torch a partir do O2R completo da ROM BR
(ver guia genérico, Passo 6) — o script assume que as 76 entradas já foram
copiadas para o pack.

---

## 5. Validação rápida

```python
import zipfile, struct
z = zipfile.ZipFile('bk_pt-BR.o2r')
assert z.testzip() is None
assert z.read('version')[1:] == struct.pack('>I', 0xB1CC3F73)
assert len([n for n in z.namelist() if '/dialog/' in n]) == 748
assert len([n for n in z.namelist() if 'ASSET_6EB' in n]) == 76
```

Checklist completo no guia genérico (Passo 8).

---

## 6. Limitações conhecidas

- **Modelos com texto pintado** (placas 3D, outdoors) não são traduzidos. A tentativa
  de sobrepor só os sub-assets divergentes gerou família inconsistente e afetou o
  boot — por isso o pack contém apenas famílias completas (texto + fonte).
- **Nomes de fase no menu de estatísticas** dependem de sprites/modelos e não estão
  no pack (mesma limitação relatada pelo autor da tradução espanhola).
- O pack pressupõe base **US v1.1** (`bk.o2r` gerado de `ded6ee16…`), que é a base
  deste port. Em outra base, o selo PAL continua válido, mas confira o remap.
