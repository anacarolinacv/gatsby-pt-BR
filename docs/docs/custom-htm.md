---
title: Personalizar html.js
---
Gatsby utiliza um componente React para a renderização do servidor `<head>` e outras partes do HTML fora do aplicativo principal do Gatsby.

A maioria dos sites deveria utilizar o `html.js` padrão enviado com o Gatsby. Contudo, se você precisar customizar seu site em html.js, copie o padrão em seu código o executando:

```shell
cp .cache/default-html.js src/html.js
```
E depois faça as modificações que você necessita.
Se você precisar adicionar html customizado no `<head>` ou `<footer>` em cada página de seu site, você pode utilizar `html.js`.

> Personalizar o `html.js` é uma alternativa para quando o uso das APIs apropriadas não estiverem disponíveis no `gatsby-ssr.js`. Considere usar [`onRenderBody`](/docs/ssr-apis/#onRenderBody) or [`onPreRenderHTML`](/docs/ssr-apis/#onPreRenderHTML) ao invés do método acima.
> Como consideração adicional, a customização do `html.js` não é suportada dentro de um Tema Gatsby. Use o método de API mencionado acima.
## Props Necessárias
Note: Os vários _props_ que são renderizados na página são necessários, por exemplo: 
`headComponents`, `preBodyComponents`, `body`, e `postBodyComponents`. 

## Inserindo HTML no `<head>`
Qualquer coisa que você renderizar no componente `html.js` não será "ativo" no cliente como outros componentes. Se
você quiser tornar mais dinâmica a atualização de sua `<head>`, nós recomendamos o uso de: 
[React Helmet](/packages/gatsby-plugin-react-helmet/)

## Inserir HTML no `<footer>`
Se você quiser inserir html personalizado no _footer_, o caminho preferido para se fazer isso é pelo 'html.js'. Se você está escrevendo um plugin, considere o uso da prop  `setPostBodyComponents` no [Gatsby SSR API](/docs/ssr-apis/).

## Target container 
Se você ver o erro: `Uncaught Error: _registerComponent(...):Target container is not a DOM element.` isso significa que o seu `<html.js>` está com a ausência da solicitação "target container". Dentro do seu `<body>` você deve ter uma div com o id `___gatsby` como:
```jsx:title=src/html.js
<div
  key={`body`}
  id="___gatsby"
  dangerouslySetInnerHTML={{ __html: this.props.body }}
/>
```
## Adicionado o JavaScript personalizado
Você pode adicionar JavaScript personalizado ao seu documento HTML usando o atributo [dangerouslySetInnerHTML] (https://pt-br.reactjs.org/docs/dom-elements.html#dangerouslysetinnerhtml) do React.

```jsx:title=src/html.js
<script
  dangerouslySetInnerHTML={{
    __html: `
            var name = 'world';
            console.log('Hello ' + name);
        `,
  }}
/>
```
