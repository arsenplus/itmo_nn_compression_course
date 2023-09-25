# itmo_nn_compression_course
This repo contains homework assignments for "Deep Learning Models Compression" course at ITMO University (2023)

## Pruning
- в качестве инструмента для прунинга была выбрана библиотека [TextPruner](https://github.com/airaria/TextPruner) - в ней реализован прунинг трансформеров-энкодеров.
- убрали по 4 головы внимания из каждого слоя, было 12 - стало 8
- уменьшили размерность ffn-слоев в полтора раза
- существенно сократили словарь и матрицу эмбеддингов, оставив только те токены, которые хотя бы один раз встречаются в калибровочной выборке. Такой подход очень удобен, если нужно адаптировать сетку под конкретный язык/домен
- Итог: скорость инференса возросла примерно в 2 раза, ROC AUC упал с ~96% до ~89%, размер сократился с 711 Мб до 322 Мб

## Quantization
- в качестве инструмента использовали [встроенный функционал PyTorch](https://pytorch.org/tutorials/intermediate/dynamic_quantization_bert_tutorial.html) - pst-training динамическое квантование
- Квантовали в int8 - размер на жестком диске упал с 711 Мб до 455 Мб
- ROC AUC упал незначительно - с ~96% до ~95%
- В качестве минуса: встроенный инструмент Torch не позволяет получающейся модели работать с CUDA-бэкендом. Квантование в Int8 не даст прирост по скорости инференса на многих видеокартах, но на CPU это может дать значительнный прирост

## Карточка модели
https://huggingface.co/Den4ikAI/ruBert-base-qa-ranker/tree/main

## Базовые замеры
<img width="688" alt="Screenshot 2023-09-25 at 17 20 54" src="https://github.com/arsenplus/itmo_nn_compression_course/assets/56270907/9101eeb1-5f14-44c9-a6cf-b4f0a76b4b99">

## Замеры после прунинга
<img width="686" alt="Screenshot 2023-09-25 at 17 22 16" src="https://github.com/arsenplus/itmo_nn_compression_course/assets/56270907/207675ca-dff7-4918-9662-1db4b275b0f5">


