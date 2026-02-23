# 💣 Bombardeio 1000Grau

> **Versão:** 1.0.0  **Compatibilidade:** Python 2.7 & Python 3.6+

> **Autor:** Carlos de Lima Junior (CJ) **Data: 19/02/2026**

> **Contexto:** Avaliação à Distância 1 (AD1) - Programação com Interfaces Gráficas

---

## 📖 Sobre o Projeto

**Bombardeio 1000Grau** é uma implementação robusta de um jogo de estratégia em terminal inspirado no clássico *Bomberman*, desenvolvido com foco estrito em **Lógica de Programação**, **Orientação a Objetos** e **Persistência de Dados**.

O diferencial deste projeto é seu sistema de **Dificuldade Dinâmica Adaptativa**, onde o jogo "aprende" com o desempenho do jogador, ajustando a densidade do mapa e a agressividade dos inimigos automaticamente entre as partidas.

### 🚀 Features Exclusivas

* **Cross-Version Core:** Código arquitetado para rodar nativamente tanto em ambientes legados (Python 2.7) quanto modernos (Python 3.x) sem modificações externas.
* **Engine de Input 'Zero-Latency':** Utiliza bibliotecas de baixo nível (`msvcrt` no Windows, `termios/tty` no Linux/Mac) para capturar teclas instantaneamente, sem necessidade de pressionar Enter.
* **Persistência Inteligente:** Utiliza arquivos JSON para manter estado entre sessões e calcular estatísticas globais que influenciam a geração procedural dos próximos mapas.
* **Renderização Otimizada:** Sistema de limpeza de buffer de terminal para criar uma experiência visual fluida, próxima a uma taxa de quadros de jogos reais.

---

## 🛠️ Guia de Instalação e Configuração

Para executar este projeto, você precisa ter um interpretador Python instalado. Abaixo, o guia para configurar seu ambiente.

### 1. Verificando a Instalação Atual

Abra seu terminal e digite:

```bash
python --version
# ou
python3 --version
```

### 2. Instalação Manual (Por Sistema Operacional)

#### 🪟 Windows

1. Acesse [python.org/downloads](https://www.python.org/downloads/).
2. Baixe a versão mais recente (Python 3.x) ou a 2.7.18 (se necessário legado).
3. **Importante:** Na instalação, marque a caixa **"Add Python to PATH"**.

#### 🐧 Linux

**Debian/Ubuntu/Mint (`apt`):**

```bash
sudo apt update
sudo apt install python3      # Para Python 3
# Para Python 2 (em distros antigas ou via PPA):
sudo apt install python2
```

**Fedora/RHEL/CentOS (`dnf`):**

```bash
sudo dnf install python3      # Para Python 3
# O Python 2 foi removido nos repositórios recentes. Instalação via fonte recomendada (ver seção de compilação abaixo).
```

**Arch Linux/Manjaro (`pacman`):**

```bash
sudo pacman -S python         # Instala a última versão estável (3.x)
sudo pacman -S python2        # Instala Python 2.7 (mantido no repositório extra/AUR)
```

**OpenSUSE (`zypper/rpm`):**

```bash
sudo zypper install python3
```

#### 🍎 macOS

Recomendamos usar o **Homebrew**:

```bash
brew install python           # Instala Python 3
brew install python@2         # Instala Python 2 (se disponível no tap)
```

---

## 🎮 Como Executar

Navegue até a pasta correspondente à versão do seu interpretador:

**Para Python 3:**

```bash
cd "Code Python 3"
python3 main.py
# ou no Windows: python main.py
```

**Para Python 2:**

```bash
cd "Code Python 2"
python2.7 main.py
```

---

## 🧠 Arquitetura e Customização (Para Desenvolvedores)

O projeto segue o padrão MVC (Model-View-Controller) simplificado:

* **`main.py` (Controller/View):** Gerencia o loop principal, inputs e renderização.
* **`classes.py` (Model):** Contém a lógica de negócio (`Mapa`, `Inimigo`, `Bomba`).
* **`persistencia.py` (Data):** Abstração de I/O para arquivos JSON.

### ⚙️ Ajustando o Balanceamento (Hot-Tweak)

Você pode customizar as regras do jogo editando diretamente as propriedades estáticas em `persistencia.py` ou `classes.py`.

#### 1. Alterar a Área de Explosão

No arquivo `persistencia.py`, localize o dicionário `PADRAO_GLOBAL`:

```python
"alcance_bomba": 2,          # Aumente para explosões maiores
"tempo_detonacao_bomba": 3,  # Diminua para explosões mais rápidas
```

#### 2. Modificar a Inteligência dos Inimigos

No arquivo `classes.py`, método `processar_turno_inimigos`:

```python
# Lógica de Spawn
novo_base_rate = base_spawn_rate
# Altere a fórmula abaixo para permitir mais inimigos
limite_inimigos = 3 + (turno_atual // 2) 
```

#### 3. Entendendo a Dificuldade Dinâmica

O sistema lê o arquivo `estatisticas_globais.json`. Se o jogador sobrevive muito acima da média histórica, o jogo **aumenta a densidade de obstáculos** na próxima partida para dificultar a fuga:

```python
# Trecho em persistencia.py
if ultima_sobrevivencia > (media_historica * 1.2):
    densidade_atual = min(0.7, densidade_atual + 0.05) # +5% de bloqueios
```

---

## 📂 Estrutura de Arquivos

```text
/
├── Code Python 3/          # Versão Otimizada (Recomendada)
│   ├── main.py             # Entry Point
│   ├── classes.py          # Lógica do Jogo
│   ├── persistencia.py     # Camada de Dados
│   └── sessao_atual.json   # (Gerado automaticamente)
├── Code Python 2/          # Versão Legacy (Compatibilidade Retroativa)
│   ├── main.py             # Adaptado (raw_input, encode utf-8)
│   └── ...
├── INSTRUÇÕES_JOGO.txt     # Manual do Usuário
└── README.md               # Este arquivo
```

---

## 📚 Referências de Desenvolvimento

* **Algoritmos de Grid:** Matrizes bidimensionais para representação espacial (`lista[linha][coluna]`).
* **Máquina de Estado:** Controle de fluxo via `while True` com detecção de estados de vitória/derrota.
* **Input Não-Bloqueante:** Estudo das libs `termios` (Unix) e `msvcrt` (Windows) para capturar `stdin` sem buffer.
* **Manipulação JSON:** Serialização de objetos para persistência leve.

---

> *"A complexidade não está no código que se escreve, mas no problema que se resolve."*
