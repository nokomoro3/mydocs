# 画像処理

## PythonでOCR(Tesseract + PyOCR)

- https://rightcode.co.jp/blog/information-technology/python-tesseract-image-processing-ocr

## pdftoppm(PDFを画像に変換)

- https://atmarkit.itmedia.co.jp/ait/articles/1903/08/news039.html

## instance segmentation

- https://twitter.com/zfphalanx/status/1457946102166999046?s=12
```
- model
  mask r-cnn -> cascade/htc -> yolact -> condinst -> solo -> queryinst
- mask refine 
  refinemask, mask scoring
- augmentation
  copypaste

選択肢としては
  - end-to-end framework(mask r-cnn等)
  - unet  -> watershed
  - detection -> crop -> semantic segmentation
  +lightgbmという認識
```
