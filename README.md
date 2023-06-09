
<p align="center"><a href="https://github.com/mdhmz1/Auto-Annotate#mdhmz1"><img src="https://github.com/mdhmz1/Auto-Annotate/blob/main/asset/logos/auto-annotate-logo-transparent.png" alt="Auto-Annotate Logo" height="240"></a></p>
<h1 align="center">Auto-Annotate-BR</h1>
<p align="center">Anote imagens de todo um diretório, automaticamente, com um único comando. </p>

<p align="center"><img src="https://img.shields.io/badge/version-v1.0.0-brightgreen?style=plastic" alt="Auto-Annotate Version"> <img src="https://img.shields.io/github/repo-size/mdhmz1/Auto-Annotate?style=plastic" alt="repo size"> <img src="https://img.shields.io/github/stars/mdhmz1/Auto-Annotate?&style=social" alt="stars"></p>




Para uma explicação mais detalhada e aplicação do código, consulte [este artigo no Medium](https://medium.com/analytics-vidhya/automated-image-annotation-using-auto-annotate-tool-f8fff8ea4900).<a href="https://medium.com/analytics-vidhya/automated-image-annotation-using-auto-annotate-tool-f8fff8ea4900">![](https://img.shields.io/badge/Medium-12100E?style=for-the-badge&logo=medium&logoColor=white) </a>


**Tão simples quanto dizer: "Anote todas as placas de rua (rótulo) no dataset (diretório) do carro autônomo".**
Toda e qualquer imagem no diretório do dataset que contenha uma placa de rua é então filtrada e a anotação de segmentação é executada em um único comando.

Auto-Annotate-BR fornece anotação automática por máscaras de segmentação baseada nos rótulos definidos para os objetos nas imagens de um diretório. A ferramenta é capaz de fornecer anotações automatizadas para os rótulos definidos no dataset COCO e também oferece suporte a rótulos personalizados. Ela é construída sobre a arquitetura [Mask R-CNN](https://github.com/matterport/Mask_RCNN). 

![Working Sample: ANNOTATE CUSTOM](asset/AutoAnnotate-Working_LowRes.png)

A ferramenta de anotação automática funciona em dois modos - COCO e Personalizado.
* **Anotação de rótulo COCO** - Nenhum treinamento de modelo é necessário. Basta usar os pesos do dataset COCO. Aponte para o diretório correto e as anotações estão prontas.
* **Anotação de rótulo personalizado** - Treine o modelo para seu rótulo personalizado. Use os pesos e anote.

NOTA: Gentileza consultar o arquivo [knownIssues.md](knownIssues.md) no repositório, para checar os problemas conhecidos e sua resolução. Sinta-se à vontade para contribuir caso encontre erros/problemas durante a instalação e uso da ferramenta.

## FORMATO JSON PARA ANOTAÇÃO

### Amostra JSON: 

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

### Formato genérico JSON:
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
3. **Se for anotar objetos suportados pelo COCO Dataset:**
   Baixe pesos COCO pré-treinados (mask_rcnn_coco.h5) do [repositório oficial](https://github.com/matterport/Mask_RCNN/releases) e      armazene-os no diretório raiz (root).
   
   **Se for anotar objetos personalizados:**
   Treine Mask RCNN com os mesmos pesos.

4. Execute os comandos abaixo conforme o modo de uso - Anotar COCO ou Personalizado.
  ```bash
  python3 annotate.py annotateCoco --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
  ```
  ```bash
  python3 annotate.py annotateCustom --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
  ```

5. Confira as anotações no /caminho/para/o/diretorio/de/imagens/ como especificado acima.



### Anotando no MS COCO
Use pesos pré-treinados para MS COCO. Podemos executar diretamente da linha de comando da seguinte forma:
```
# Anotar rótulo definido pelo COCO
python3 annotate.py annotateCoco --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
```
Nota: --label=rotulo_a_anotar deve estar de acordo com os rótulos do COCO dataset (em inglês  - a internacionalização deve ser feita em outra etapa). Consulte [COCO Dataset](https://cocodataset.org/) para mais detalhes.


### Anotando em imagens personalizadas

Use pesos pré-treinados para o rótulo personalizado. Podemos executá-lo diretamente da linha de comando da seguinte forma:

```
# Annotate Custom
python3 annotate.py annotateCustom --image_directory=/caminho/para/o/diretorio/de/imagens/ --label=rotulo_a_anotar --weights=/caminho/para/os/pesos.h5 --displayMaskedImages=False
```
Nota: --label=rotulo_a_anotar deve ser um rótulo para o qual tenha sido fornecido peso (ex: 'cow').


### Treinando seu próprio dataset

Leia o post original [no blog de Waleed Abdulla](https://engineering.matterport.com/splash-of-color-instance-segmentation-with-mask-r-cnn-and-tensorflow-7c761e238b46) onde ele explicou o processo, desde a anotação de imagens até o treinamento e o uso dos resultados em um aplicativo de exemplo.

Abaixo O uso de train.py, que é uma versão modificada de balloon.py escrita por Waleed para suportar apenas a fase de treinamento. [Muhammad Hamzah](https://github.com/mdhmz1) é o autor da modificação.
```
    # Treinar um novo modelo a partir de pesos COCO pretreinados
    python3 customTrain.py train --dataset=/caminho/para/dataset/personalizado/ --weights=coco

    # Retomar o treinamento de um modelo já iniciado:
    python3 customTrain.py train --dataset=/caminhp/para/dataset/personalizado --weights=last
```

### :clap: Apoiadores

### :twisted_rightwards_arrows: Forkers 
[![Forkers repo roster for @mdhmz1/Auto-Annotate](https://reporoster.com/forks/dark/mdhmz1/Auto-Annotate)](https://github.com/mdhmz1/Auto-Annotate/network/members)

##

[🤝 NOSSO BLOG](https://www.voxleone.com/blog/)

##
