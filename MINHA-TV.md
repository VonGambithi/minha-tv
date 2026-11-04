# Minha TV (versão para LG webOS)

Tela inicial personalizada para TVs LG com webOS, baseada no projeto [Free My TV](https://github.com/simbasjungles/free-my-tv) (licença MIT).

O que esta versão faz:

- Imagem do título selecionado ocupa a tela inteira ao fundo.
- Abas Explorar, Filmes e Séries no topo, com o título e o subtítulo logo abaixo.
- Uma fileira de cartões do TMDB (até 30) que rola para a direita.
- Uma fileira de apps na base da tela, só com logos, na ordem: Bell Fibe TV, Crave, Smarters, Disney+, Prime Video, Netflix, YouTube, Spotify e Web Browser. Seis aparecem de uma vez, e a fileira rola para a direita.
- O botão de apps (no alto, à direita) abre "Todos os apps", com todos os apps da TV.
- O nome do app é "Minha TV".

Limites do webOS: o app **não** consegue abrir sozinho quando a TV liga, nem voltar sozinho quando você sai de outro app. Isso só funciona com root na TV, e este projeto não faz isso.

## O que você precisa

- TV LG com webOS 3 ou mais novo.
- Um computador na mesma rede Wi-Fi, com **Python 3** (no Mac já vem).
- Uma conta grátis em https://www.themoviedb.org para pegar a chave do TMDB.

## Passo a passo

### 1. Deixe a TV pronta

1. Na TV, abra a **LG Content Store** e instale o app **Developer Mode**.
2. Entre com uma conta grátis de desenvolvedor LG (https://webostv.developer.lge.com).
3. Ligue **Dev Mode Status** (a TV reinicia) e depois ligue **Key Server**.
4. Anote o **endereço IP** da TV que o app mostra.

### 2. Baixe o projeto

```bash
git clone https://github.com/VonGambithi/minha-tv.git
cd minha-tv
```

### 3. Crie a configuração (sua chave do TMDB)

1. No themoviedb.org, abra Configurações, API, e copie a **chave da API**.
2. Copie o arquivo de exemplo:

   ```bash
   cp config.example.properties config.properties
   ```

3. Abra `config.properties` e preencha:

   ```
   tmdb_key=COLE_SUA_CHAVE_AQUI
   tmdb_region=CA
   ```

   `tmdb_region` é o país dos serviços de streaming (CA, US, BR e assim por diante).

O `config.properties` fica só no seu computador. Ele está no `.gitignore` e não deve ser enviado para o GitHub.

### 4. Leia a lista de apps da TV

Em webOS 6 ou mais novo, a TV não mostra ao app quais apps estão instalados. O computador pede essa lista:

```bash
python3 webos/apps.py 192.168.1.50
```

Troque o IP pelo da sua TV. **Olhe a TV e aceite o aviso "Permitir"** com o controle em até cerca de 90 segundos. Sem isso, os apps como Bell Fibe TV, Crave e Smarters não aparecem. Rode de novo sempre que instalar ou remover apps na TV.

### 5. Compile e instale

```bash
python3 webos/build.py
python3 webos/install.py 192.168.1.50
```

Na primeira instalação, o Developer Mode pede a **senha (passphrase)** que o app Developer Mode mostra na TV. Digite no seu terminal. Não compartilhe essa senha com ninguém.

Pronto: o app "Minha TV" abre na TV.

## Dicas

- O Developer Mode dura cerca de 40 dias. Abra o app Developer Mode de tempos em tempos e aperte **EXTEND**.
- Para remover: `python3 webos/install.py 192.168.1.50 --uninstall`.
- Se a TV não responder na instalação (erro de tempo esgotado, porta 9922), ligue a TV e confira se o Developer Mode está ativo.
- Para ver só no computador, sem TV: `python3 webos/build.py --preview` e abra `out/webos/index.html`.

## Segurança

- Nunca publique `config.properties` (tem a sua chave do TMDB).
- Nunca publique a sua chave de acesso do Developer Mode (fica em `~/.ssh/`).
- Os arquivos `webos-apps.local.json` e `webos-icons.local/` são da sua TV e já estão ignorados pelo git.

## Créditos e logos

- Projeto base: [Free My TV](https://github.com/simbasjungles/free-my-tv), MIT. A parte de LG parte do trabalho de [@danielthejoker18](https://github.com/danielthejoker18/FreeMyTvWebOS).
- Dados de filmes e séries: TMDB. Este produto usa a API do TMDB, mas não é endossado nem certificado por ele.
- Os logos de Bell, Crave, Disney+, Netflix e outros são marcas dos seus donos e aparecem aqui só para identificar os apps.
