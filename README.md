
<p align="center"><a href="https://github.com/mdhmz1/Auto-Annotate#mdhmz1"><img src="https://github.com/mdhmz1/Auto-Annotate/blob/main/asset/logos/auto-annotate-logo-transparent.png" alt="Auto-Annotate Logo" height="240"></a></p>
<h1 align="center">Auto-Annotate-BR</h1>
<p align="center">Anote imagens de todo um diretório, automaticamente, com um único comando. </p>

<p align="center"><img src="https://img.shields.io/badge/version-v1.0.0-brightgreen?style=plastic" alt="Auto-Annotate Version"> <img src="https://img.shields.io/github/repo-size/mdhmz1/Auto-Annotate?style=plastic" alt="repo size"> <img src="https://img.shields.io/github/stars/mdhmz1/Auto-Annotate?&style=social" alt="stars"></p>




Para uma explicação mais detalhada e uso do código, consulte este [artigo no Medium](https://medium.com/analytics-vidhya/automated-image-annotation-using-auto-annotate-tool-f8fff8ea4900).

<a href="https://medium.com/analytics-vidhya/automated-image-annotation-using-auto-annotate-tool-f8fff8ea4900">![](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white) </a>


**Tão simples quanto dizer: "Anote todas as placas de rua (rótulo) no dataset (diretório) do carro autônomo" e PRONTO!**
Toda e qualquer imagem no diretório do dataset contendo uma placa de rua é filtrada e a anotação de segmentação é executada em um único comando.

Auto-Annotate-BR fornece anotação automática por máscaras de segmentação baseada nos rótulos definidos para os objetos nas imagens de um diretório. A ferramenta é capaz de fornecer anotações automatizadas para os rótulos definidos no dataset COCO e também oferece suporte a rótulos personalizados. Ela é construída sobre a arquitetura [Mask R-CNN](https://github.com/matterport/Mask_RCNN). 

![Working Sample: ANNOTATE CUSTOM](asset/AutoAnnotate-Working_LowRes.png)

A ferramenta de anotação automática funciona em dois modos
* **Anotação de rótulo COCO** - NENHUM TREINAMENTO DE NECESSÁRIO. Basta usar os pesos do dataset COCO. Aponte para o diretório corretos e as anotações estão prontas.
* **Anotação de rótulo personalizado** - Treine o modelo para o rótulo personalizado. Use os pesos e anote.

NOTE: Please refer to [knownIssues.md](knownIssues.md) file in the repo for known issues and their resolution. Please feel free to contribute in case of any errors/issues arising during the installation and usage of the tool.

## Formato JSON para anotação

### AMOSTRA JSON: 

```json
[
  {
    "filename": "bird_house_in_lawn.jpg",
    "id": 1,
    "label": "Bird House",
    "bbox": [ 111.5, 122.5, 73, 66 ],
    "segmentation": [
      [ 167, 188.5, 174, 185.5, 177.5, 181, 177.5, 157, 183.5, 154, 184.5, 149, 159, 124.5, 150, 122.5, 
        142, 124.5, 131, 131.5, 111.5, 150, 111.5, 156, 116.5, 162, 116.5, 184, 121, 188.5, 167, 188.5
      ]
    ]
  }
]
```

### FORMATO GENÉRICO JSON:
```
[
  {
    "filename": image_file_name,
    "id": id_of_the_image,
    "label": label_to_search_and_annotate,
    "bbox": [ x, y, w, h ], -- x,y coordinate of top left point of bounding box
                            -- w,h width and height of the bounding box
                            Format correspond to coco json response format for bounding box
    "segmentation": [
      [ x1, y1, x2, y2,...] -- For X,Y belonging to the pixel location of the mask segment
                            -- Format correspond to coco json response format for segmentation
    ]
  }
]
```

###
IMAGEM ORIGINAL            |  IMAGEM 'MASCARADA'
:-------------------------:|:-------------------------:
![](asset/bird_house_in_lawn.jpg)  |  ![](asset/bird_house_in_lawn_masked.jpg)


## Instalação
1. Clone este repositório.

2. Instale as dependências.
   ```bash
   pip3 install -r requirements.txt
   
   ```
3. **Se for anotar objetos suportados pelo COCO Dataset**
   Baixe pesos COCO pré-treinados do COCO (mask_rcnn_coco.h5) da [releases page](https://github.com/matterport/Mask_RCNN/releases) e    armazene-os no diretório raiz (root).
   **Sefor anotar objetos personalizados**
   Treine Mask RCNN e use esses pesos.

4. Execute os comandos abaixo com base no modo.
  ```bash
  python3 annotate.py annotateCoco --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
  ```
  ```bash
  python3 annotate.py annotateCustom --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
  ```

5. Veja as anotações no /caminho/para/o/diretorio/de/imagens/ como especificado acima.


## Anotando no MS COCO
Use pesos pré-treinados para MS COCO. Podemos executar diretamente da linha de comando da seguinte forma:
```
# Anotar rótulo definido pelo COCO
python3 annotate.py annotateCoco --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
```
Nota: --label=rotulo_a_anotar deve estar de acordo com os rótulos do COCO dataset (em inglês  - a internacionalização deve ser feita em outra etapa).
Consulte [COCO Dataset](https://cocodataset.org/) para mais detalhes.

## Anotando em imagens personalizadas
Use pesos pré-treinados para o rótulo personalizado. Podemos executá-lo diretamente da linha de comando da seguinte forma:

```
# Annotate Custom
python3 annotate.py annotateCustom --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
```
Nota: --label=rotulo_a_anotar deve ser um rótulo para o qual tenha sido fornecido peso (ex: 'cow').


## Treinando seu próprio dataset

Leia o post original de Waleed Abdulla [post do blog sobre o exemplo do 'respingo de cor'](https://engineering.matterport.com/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46) onde ele explicou o processo desde a anotação de imagens até o treinamento e o uso dos resultados em um aplicativo de exemplo.

Abaixo O uso de train.py, que é uma versão modificada de balloon.py escrita por Waleed para suportar apenas a parte de treinamento. [Muhammad Hamzah](https://github.com/mdhmz1) é o autor da modificação.
```
    # Treinar um novo modelo a partir de pesos COCO pretreinados
    python3 customTrain.py train --dataset=/caminho/para/dataset/personalizado/ --weights=coco

    # Retomar o treinamento de um modelo já iniciado:
    python3 customTrain.py train --dataset=/caminhp/para/dataset/personalizado --weights=last
```

## :clap: Supporters

### :star: Stargazers
[![Stargazers repo roster for @mdhmz1/Auto-Annotate](https://reporoster.com/stars/dark/mdhmz1/Auto-Annotate)](https://github.com/mdhmz1/Auto-Annotate/stargazers)
### :twisted_rightwards_arrows: Forkers 
[![Forkers repo roster for @mdhmz1/Auto-Annotate](https://reporoster.com/forks/dark/mdhmz1/Auto-Annotate)](https://github.com/mdhmz1/Auto-Annotate/network/members)

##

[🤝 NOSSO BLOG](https://www.voxleone.com/blog/)

##
