# TAKVox

**Canais de voz PTT e chamadas de vídeo para o ATAK-CIV, em um servidor de mídia que você mesmo hospeda (LiveKit).** Sem conta em nuvem e sem serviço de terceiros: o plugin fala com o seu próprio servidor.

*English: [README.md](README.md)*

> Projeto independente da comunidade. Sem vínculo ou endosso do TAK Product Center ou de qualquer fornecedor de TAK.

## Download

Baixe o plugin assinado em **[Releases](../../releases/latest)**: `ATAK-Plugin-TakVox-<versão>--5.6.0-civ-release.apk`.
Ele é assinado pelo Third Party Pipeline do TAK.gov, então carrega no **ATAK-CIV 5.6 da Play Store**.

1. Instale o APK no aparelho Android (`adb install -r <arquivo>.apk`, ou abra o arquivo no aparelho).
2. Abra o ATAK, aceite o aviso de carregar o plugin e abra o **TAKVox** na barra de ferramentas.
3. Em **Configurações**, informe a URL do servidor de tokens e toque em **Testar**.

> Se você instalou antes uma versão compilada localmente (debug), desinstale primeiro: a assinatura é outra.

Você também precisa do servidor. Veja **[server/](server/)** para montar o LiveKit e o serviço de tokens.

## Funcionalidades

- **Canais de voz.** Cada canal é uma sala do LiveKit. Os canais fixos são definidos pelo administrador do servidor, com restrição opcional por usuário e opção "só voz".
- **Criar e apagar canais pelo app.** Canais criados pela equipe podem ter senha opcional. Canais abertos podem ser apagados por qualquer usuário autenticado, com confirmação; canais com senha pedem a senha do canal. Canais fixos não podem ser apagados pelo app.
- **Dois modos:** **PTT** clássico (segurar ou alternar) e modo **Chamada** (microfone aberto mais vídeo, como videochamada).
- **Botão PTT flutuante** sobre o mapa, para continuar usando o mapa enquanto fala. Arraste para cima para travar a transmissão contínua e para baixo para destravar.
- **PTT por hardware:** botão físico ou Bluetooth via intents, inclusive as ações de PTT de aparelhos rugged, configuráveis em Configurações sem recompilar.
- **Lista de participantes** em tempo real, com quem está falando. Toque num participante para centralizar o mapa no marcador dele.
- **Vídeo:** publica a câmera (traseira por padrão, com troca), mostra o vídeo dos outros em um palco com a sua própria imagem e uma faixa de participantes.
- **Sem login extra.** O plugin usa o **certificado cliente que o ATAK já tem** do seu servidor TAK (TLS mútuo). Também há chave de equipe compartilhada para testes rápidos.

## Como funciona

```
 ATAK + TAKVox --HTTPS + certificado cliente do TAK--> token-service --(JWT)--+
        |                                                                     |
        +-------------- WebRTC (áudio/vídeo) -----------------> LiveKit <-----+
```

O serviço de tokens só entrega um token a quem apresenta certificado assinado pela CA do seu servidor TAK. Ele não conversa com o servidor TAK, então não depende de qual servidor TAK você usa. Foi testado com o OpenTAKServer.

## Limitações conhecidas

- **Microfone em segundo plano:** com o ATAK minimizado ou a tela desligada, o Android 14+ pode calar a transmissão. Receber funciona; para falar, mantenha o ATAK na tela.
- **Microfone pré-aquecido** (padrão) mantém o indicador de microfone do Android aceso enquanto conectado, mas a trilha fica publicada muda. Desligue em Configurações se preferir.
- **Identidade:** quem tem certificado válido pode declarar o `uid` de outro e derrubá-lo da sala. Serve para equipe de confiança, não para ambiente hostil.
- **Rede:** o celular precisa alcançar o seu servidor LiveKit (VPN ou IP público). Multicast não é usado.
- **Não incluído:** câmera térmica/UVC, chat de texto, chamadas privadas, indicador de fala sobre o marcador no mapa.

## Requisitos
- ATAK-CIV 5.6 no Android, microfone (e câmera para vídeo).
- Um servidor LiveKit e o serviço de tokens de [server/](server/).

## Apoie o projeto

Se o TAKVox for útil para você, pode apoiar o desenvolvimento:

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-doar-FFDD00?logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/tiagoufbao)

Ou por Pix: `tiagoufba@gmail.com`

## Termos de uso
O plugin é distribuído gratuitamente, somente em formato binário. O código-fonte do plugin não é publicado. Você pode usá-lo, mas não pode redistribuir, modificar nem revender sem autorização. Fornecido "como está", sem garantia. O ATAK e seu SDK têm termos próprios. O kit do servidor em [server/](server/) tem licença própria.

## Software de terceiros
O plugin inclui o **LiveKit Android SDK** (Apache-2.0, https://github.com/livekit/client-sdk-android) e o **WebRTC** (BSD-3-Clause). O código-fonte está nesses links.
