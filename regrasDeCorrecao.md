# Referência de item revogado
Não incluir a classe `leiRef`, apenas a tag `<del>{texto}</del>`

# Regras de uso das classes leiRef, leiRef2, 3 e 4
- leiRef: referências cruzadas
- leiRef2: Revogados, alterados, Vide - fica pequenininho e há quebra de linha. É botão de toggle do revogado - marcar de escuro o botão de revogado!
- leiRef3: Quando acontecer de (Revogado) ou (Vetado) e tiver a informação de lei
  - Ex.: "§ 2.º (Revogado pela Lei...)"
- leiRef4: Revogados, alterados - fica pequenininho e há quebra de linha, mas fica sumido. Aparece só no botão de revogados

# Palavra "Vigência" em leiRef
Apagar os `<span></span>` que contiver essa palavra, tanto "Vigência", "(Vigência)" ou "Produção de efeitos".
  Ex: `<span class="leiRef">Vigência</span>`

# Alerta para as leis que não possuem texto compilado
Algumas partes podem fica com classe "revogado" quando não é verdade, sendo necessário está alerta. Se for revogado um artigo inteiro, deverá sinalizar para não pular artigo.

# Padrão de disposítivos
- Paragráfos: § 6.º
- Artigos: Art. 6.º
- A partir do 10 em diante é apenas Art. 10. e § 10.