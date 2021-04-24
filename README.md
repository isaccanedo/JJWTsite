# JJWTsite
:cat: # Informações e documentação sobre a biblioteca JJWT Open Source

# 1. Autenticação vs. Autenticação de Token
O conjunto de protocolos que um aplicativo usa para confirmar a identidade do usuário é a autenticação. Os aplicativos têm tradicionalmente uma identidade persistente por meio de cookies de sessão. Esse paradigma se baseia no armazenamento de IDs de sessão do lado do servidor, o que força os desenvolvedores a criar um armazenamento de sessão que seja exclusivo e específico do servidor ou implementado como uma camada de armazenamento de sessão completamente separada.

A autenticação por token foi desenvolvida para resolver problemas que os IDs de sessão do lado do servidor não resolviam e não podiam. Assim como a autenticação tradicional, os usuários apresentam credenciais verificáveis, mas agora recebem um conjunto de tokens em vez de um ID de sessão. As credenciais iniciais podem ser o par de nome de usuário / senha padrão, chaves de API ou até mesmo tokens de outro serviço. (O recurso de autenticação de chave API do Stormpath é um exemplo disso.)

1.1. Por que tokens?
De forma muito simples, o uso de tokens no lugar de IDs de sessão pode reduzir a carga do servidor, otimizar o gerenciamento de permissões e fornecer ferramentas melhores para oferecer suporte a uma infraestrutura distribuída ou baseada em nuvem. No caso do JWT, isso é feito principalmente por meio da natureza sem estado desses tipos de tokens (mais sobre isso a seguir).

Os tokens oferecem uma ampla variedade de aplicativos, incluindo: esquemas de proteção Cross Site Request Forgery (CSRF), interações OAuth 2.0, IDs de sessão e (em cookies) como representações de autenticação. Na maioria dos casos, os padrões não especificam um formato particular para tokens. Aqui está um exemplo de um token CSRF Spring Security típico em um formato HTML:

```
<input name="_csrf" type="hidden" 
  value="f3f42ea9-3104-4d13-84c0-7bcb68202f16"/>
```

Se você tentar postar esse formulário sem o token CSRF correto, obterá uma resposta de erro e isso é a utilidade dos tokens. O exemplo acima é um token “burro”. Isso significa que não há nenhum significado inerente a ser extraído do token em si. É aqui também que os JWTs fazem uma grande diferença.

2. O que há em um JWT?
JWTs (pronuncia-se “jots”) são strings seguras com URL, codificadas, criptograficamente assinadas (às vezes criptografadas) que podem ser usadas como tokens em uma variedade de aplicativos. Aqui está um exemplo de um JWT sendo usado como um token CSRF:

```
<input name="_csrf" type="hidden" 
  value="eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJlNjc4ZjIzMzQ3ZTM0MTBkYjdlNjg3Njc4MjNiMmQ3MCIsImlhdCI6MTQ2NjYzMzMxNywibmJmIjoxNDY2NjMzMzE3LCJleHAiOjE0NjY2MzY5MTd9.rgx_o8VQGuDa2AqCHSgVOD5G68Ld_YYM7N7THmvLIKc"/>
```

Nesse caso, você pode ver que o token é muito mais longo do que em nosso exemplo anterior. Assim como vimos antes, se o formulário for enviado sem o token, você receberá uma resposta de erro.

Então, por que JWT?

O token acima é assinado criptograficamente e, portanto, pode ser verificado, fornecendo prova de que não foi adulterado. Além disso, os JWTs são codificados com uma variedade de informações adicionais.

Vejamos a anatomia de um JWT para entender melhor como extraímos toda essa bondade dele. Você deve ter notado que existem três seções distintas separadas por pontos (.):
