# Publicando-App-Ionic
Roteiro que realizo para publicar meu apps feitos em Ionic nas lojas.

#Publicando App Ionic na Google Play

## Passo 1 - Verificar se as informações do projeto estão ok
- Setar as informações (nome do app, versão e etc) no arquivo config.xml
- Verificar se já gerei as imagens com ionic cordova resources -f
- Após gerar as imagens, ajustar a logo, pois o ionic as vezes gera as imagens cortadas

## Passo 2 - Gerar APK não assinado
Entra na pasta do app 
ex: 

```sh
cd "C:\Projeto\MeuApp"
```

Gerar o apk não assinado com o comando abaixo
```sh
ionic cordova build android --release --prod
```

### Observação
Caso você tenha o seguinte problema:

> What went wrong: Execution failed for task ':app:mergeReleaseResources'.
> java.util.concurrent.ExecutionException: com.android.builder.internal.aapt.v2.Aapt2InternalException: AAPT2 aapt2-3.3.0-5013011-windows Daemon #2: Unexpected error during compile 'E:\Unnati\WorkSpace\gita\platforms\android\app\src\main\res\drawable-land-xxxhdpi\screen.png', attempting to stop daemon. This should not happen under normal circumstances, please file an issue if it does.

Procure o arquivo **build.gradle** e adicione isso:
```
allprojects {
    // Workaround for https://issuetracker.google.com/117900475
    // Remove when upgrading to AGP 3.4 or higher.
    configurations.matching { it.name == '_internal_aapt2_binary' }.all { config ->
        config.resolutionStrategy.eachDependency { details ->
            details.useVersion("3.5.0-alpha03-5252756")
        }
    }
}
```

## Passo 3 - Gerar um Key (somente 1 vez)
Ao publicar o App pela primeira vez, é necessário gerar uma key(chave). Nas próximas versões é só utilizar a key(chave) gerada.

OBS: Não executar os comandos abaixo, caso você já tenha uma Key

Caso a máquina não esteja configurada corretamente, acessar a pasta do JAVA -> JRE -> BIN
```sh
cd "C:\Program Files (x86)\Java\jre1.8.0_144\bin"
```

Para criar a chave utilize o comando abaixo:

```sh
.\keytool.exe -genkey -v -keystore "C:\Projeto\MeuApp\minha.keystore" -alias minhaKey -keyalg RSA -keysize 2048 -validity 10000
```

Guarde a senha!
senha: senha123


## Passo 4 - ASSINAR APK
Caso sua máquina não esteja configurada corretamente, entre na pasta abaixo para executar o comando.

```sh
cd "C:\Program Files\Java\jdk1.8.0_144\bin\"
```

Comando que deve ser executado, lembre se informar a key no final do comando

```sh
.\jarsigner.exe -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore "C:\Projeto\MeuApp\minha.keystore" "C:\Projeto\MeuApp\platforms\android\build\outputs\apk\android-release-unsigned.apk" minhaKey 
```

## Passo 5- EXECUTAR ZIPALIGN
Caso a máquina não esteja configurada corretamente, entre na pasta 

```sh
cd "C:\Program Files (x86)\Android\android-sdk\build-tools\26.0.2\"
```

Execute o seguinte comando
```sh
.\zipalign.exe -v 4 "C:\Projeto\MeuApp\platforms\android\build\outputs\apk\android-release-unsigned.apk" "C:\Projeto\MeuApp\MeuApp-0.0.2.apk"
```


### Observações
O ideal é configurar as variaveis de ambiente, assim você evita de ficar navegando até essas pastas

# VEJA TAMBÉM
## Grupo de Estudo no Telegram
- [Participe gratuitamente do grupo de estudo](https://t.me/blogilovecode)

## Cursos baratos!
- [Meus cursos](https://olha.la/udemy)

## Fique ligado, acesse!
- [Blog ILoveCode](https://ilovecode.com.br)

## Novidades, cupons de descontos e cursos gratuitos
https://olha.la/ilovecode-receber-cupons-novidades
