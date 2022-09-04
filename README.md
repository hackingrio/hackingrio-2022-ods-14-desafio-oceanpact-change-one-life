[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=8362866&assignment_repo_type=AssignmentRepo)
>Esse é um arquivo de template e tem como o intuito prover uma breve apresentação de seu projeto para avaliadores e mentores. Sinta-se à vontade para editar os tópicos e títulos de acordo com seu gosto ou até mesmo para deletá-lo ou sobrescreve-lo caso o queira. Cheque também o arquivo [repositorio.md](https://github.com/hackingrio/template/blob/master/repositorio.md) para saber o que acontecerá com esse repositório depois que o evento acabar.

# NFTurtle - 2022

### Change one life/ ODS 14 - Vida na água

### Apresentação

Somos o time *Change one life* e fomos motivados pela ODS 14 porque nos pareceu um desafio divertido de trabalhar em cima, além de ser um tema super relevante.

O nosso projeto consiste em ser um app onde ao tirar uma foto da cabeça de uma tartaruga, o padrão de sua cabeça é analisada e comparada a um banco de dados. Isso é possível pois o padrão nas cabeças de tartaruga é único para cada uma, podendo ser uma forma de identificá-las. Após a análise, se a tartaruga identificada não estiver no banco de dados, um NFT da tartaruga será "mintada" com informações sobre data, hora e localização do registro e além disso, a pessoa que tirou a foto será denominada como padrinho e poderá até nomeá-la. Esta NFT será enviada para o padrinho e poderá ser convertida em um desconto para um serviço através da tecnologia de ***smart contracts*** (que é um contrato inteligente utilizado para padronizar transações entre a ***web2***, internet que conhecemos, e a ***web3***, internet que estamos construindo). O valor gerado pela NFTurtle é o desconto para usuário e um selo eco-friendly para empresas parceiras da *Oceanpact*. Esse mecanismo no final promove o reconhecimento da importância da proteção das tartarugas e do cuidado do meio ambiente como um todo. 

### O Produto

O nosso produto é inovador pois insere seu valor de proteção junto à ***web3.*** Um dos pontos positivos é a relação com a ODS que ele oferece uma recompensa financeira para o catálogamento de tartarugas e a preservação do meio ambiente. 

### Informações adicionais

O que o usuário do NFTurtle ganha? A cada foto ele acumula pontos que podem ser trocados por descontos em produtos e serviços de empresas parceiras da *Oceanpact*.

Os NFTs serão mintados na OpenSea, que é uma plataforma gratuita, o que facilitará o processo tanto para a *Oceanpact* sendo menos custoso, quanto para o usuário que não lidará com tanta burocracia.

# Protótipo de alta fidelidade

[link para o figma](https://www.figma.com/proto/ClAsjdbA7NlTiwsfTCvvGv/NFTurtle?node-id=27%3A2&scaling=scale-down&page-id=0%3A1&starting-point-node-id=5%3A2)

Segue algumas telas do nosso protótipo
<p float="left">
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela1.png" alt="drawing" width="200"/>
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela2.png" alt="drawing" width="200"/>
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela3.png" alt="drawing" width="200"/>
</p>
<p float="left">
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela4.png" alt="drawing" width="200"/>
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela5.png" alt="drawing" width="200"/>
    <img src="https://github.com/hackingrio/hackingrio-2022-ods-14-desafio-oceanpact-change-one-life/blob/master/Telas/tela6.png" alt="drawing" width="200"/>
</p>

# MVP

### Introdução

Para iniciar o processo analisamos que o “pulo do gato” para a nossa solução dar certo é o reconhecimento de do padrão da tartaruga para realizar a identificação. A organizacao  do evento ofereceu um  banco de imagens com 9 tartarugas cada uma com 10 fotos (Total 90 fotos, 9 * 10 = 90). Então para nossa solução optamos por criar um modelo de classificação de imagens usando as bibliotecas python `keras` e o `tensorflow`. 

obs: Decidimos focar no backend por conta do tempo corrido. Achamos que o mais interessante é para o MVP é ter essa tecnologia funcionando, além do fato que o frontend não é muito difícil de implementar mas demanda tempo.

### Solução

Para aproveitar ao máximo o modelo cada imagem passa por um tratamento para diversificar a identificação, toda imagem é girada 4 vezes e invertida. Assim é possível obter 8 imagens diferentes de uma única imagem. Pelo código que fizemos são 810 imagens total.

Após esse tratamento as imagens são treinadas e classificadas para cada uma das 9 tartarugas em um modelo de classificação .

Antes de fazer o treinamento retiramos 1 imagem de cada uma das nove tartarugas para testar o modelo(**Estas imagens não passaram pelo treinamento e portanto não estão no modelo**), estas estão localizadas no path: `/Fotos-cortadas-turtle/Validação`. Ao terminar o treinamento uma foto dessa pasta é escolhida para testar e é associada a ela a classe mais provável de lhe pertencer.

Input:

```python
#escolhe uma imagem para realizar um teste
pathimage = '/content/Fotos-cortadas-turtle/Validação/CM002F8.JPG'
img = tf.keras.utils.load_img(
    pathimage, target_size=(img_height, img_width)
)
img_array = tf.keras.utils.img_to_array(img)
img_array = tf.expand_dims(img_array, 0) # Create a batch

predictions = model.predict(img_array)
score = tf.nn.softmax(predictions[0])

print(
    "This image most likely belongs to {} with a {:.2f} percent confidence."
    .format(class_names[np.argmax(score)], 100 * np.max(score))
)
```

Ouput:

```
This image most likely belongs to CM002 with a 97.95 percent confidence.
```
