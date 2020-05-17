# React Native

É uma versão do React para desenvolvimento mobile.
O mesmo código roda no Android e no iOS e manipular de forma diferente.
Injeta o JS core dentro do nosso celular e roda sem transpilar.

*Arquitetura:*
JS -> Metro Bundler (packager) -> Bundle -> Bridge -> Android | iOS

Metro Bundler: seria como nosso Webpack do ReactJS.
Bundle: nosso arquivo JS.
Bridge: é a ponte entre nosso código JS em código nativo.

Sintaxe com declaração de componente similar ao web, mas não usamos HTML mas componentes próprios.

`<View>` é como a `<div>`

`<Text>` é como o `<p>`

`<TouchableOpacity>` é como `<button>`

Como são componentes próprios, nenhum vem com estilização e não é estilizado com `class` ou `id`. Precisa da propriedade `style` e a sintaxe é tudo `css`.

Yoga é uma biblioteca que converte estilo `css` em Objective-C e Java.

Expo é um SDK para usar as funcionalidades do React Native.
Não precisa instalar emulador e é só instalar nosso aplicativo dentro do Expo e ele roda no nosso celular. A desvantagem é que tem limitação sobre o código nativo. Se precisar mexer com Objective-C ou Java, não vai conseguir pelo Expo. Além disso, várias bibliotecas não tem suporte para o Expo.

Instalar SDK do Android e do iOS. (https://react-native.rocketseat.dev/)

(Instalação do XCode foi demorada para conseguir rodar o Emulador de iOS)

## Inicializando um projeto
```bash
react-native init mobile
```

Abrir no VSCode.

Excluir arquivos `.eslintrc.js`, `prettierrc.js`

Rodar o projeto
```bash
react-native run-ios --simulator "iPhone 11 Pro Max"
```

Deu erro porque XCode não estava instalado corretamente. Depois de instalar novamente pela Apple, com login e tudo mais, instalei os pods do projeto.
```bash
cd ios && pod install
```

Voltei para raiz do projeto e rodei novamente
```bash
react-native run-ios --simulator "iPhone 11 Pro Max"
```

Depois de 6 minutos, rodou 🎉🎉🎉🎉🎉 e abriu o Metro Bundle que fica ouvindo todas alterações do código e constrói um bundle que roda e atualiza a tela no emulador.

No Mac, `Cmd + R` faz o reload no emulador e `Cmd + D` abre o developer menu.

Ou no Metro bundle também pode digitar `r` para relod ou `d` para developer menu.

Deleta o arquivo `App.js` e cria um arquivo `src/index.js`.
```js
import React from 'react'
import { View } from 'react-native'

export default function App() {
  return <View />
}
```

E altera a importação dentro do `index.js` da raiz do projeto.
```js
import App from './src/index';
```


