---
published: true
title: html to pdf
layout: post
author: sw23blues
categories: Android 
tags: 
- html
- pdf
comments: true
---

**html to pdf**
<br>

iText 사용
- iText는 html이 닫는 테그가 없거나 테그가 완전하지 않으면 에러 발생, html > clean html로 변환(라이브러리 사용)
- 한글이 안나오는 경우, .ttf파일 프로젝트내에 추가
- 이미지 AbstractImageProvider로 구현


```bash

    /**
     * html to pdf
     * localMatchingFiles : 로컬이미지에서 바인딩할 img tag src 이름
     */
    public void createPdf(InputStream convertIs, String file, ArrayList<String> localMatchingFiles)  {
        try {
            Document document = new Document();
            PdfWriter writer = PdfWriter.getInstance(document, new FileOutputStream(file));
            document.open();

            XMLWorkerFontProvider fontProvider = new XMLWorkerFontProvider(XMLWorkerFontProvider.DONTLOOKFORFONTS);
            fontProvider.register("/fonts/malgun.ttf", "Malgun Gothic");

            CssAppliers cssAppliers = new CssAppliersImpl(fontProvider);

            HtmlPipelineContext htmlContext = new HtmlPipelineContext(cssAppliers);
            htmlContext.setTagFactory(Tags.getHtmlTagProcessorFactory());

            //img
            PdfImageProvider provider = new PdfImageProvider();
            provider.addLocalMatchingFile(localMatchingFiles);
            htmlContext.setImageProvider(provider); //img 태그 프로젝트 내 이미지와 매칭

            PdfWriterPipeline pdf = new PdfWriterPipeline(document, writer);
            HtmlPipeline html = new HtmlPipeline(htmlContext, pdf);
            CssResolverPipeline css = new CssResolverPipeline(new StyleAttrCSSResolver(), html); //new StyleAttrCSSResolver()

            XMLWorker worker = new XMLWorker(css, false);
            XMLParser p = new XMLParser(worker);
            p.parse(convertIs, Charset.forName("UTF-8"));

            document.close();

        } catch (Exception e) {
            DLog.d(e.getCause());
        }
    }
    
    
    class PdfImageProvider extends AbstractImageProvider {

        ArrayList<String> localMatchingFiles = new ArrayList<>();
        int i = 6;

        public void addLocalMatchingFile(List<String> fileNames) {
            localMatchingFiles.addAll(fileNames);
        }

        @Override
        public String getImageRootPath() {
            return null;
        }

        @Override
        public Image retrieve(String src) {
            Image img = super.retrieve(src);
            DLog.d("retrieve");

            if (img == null) {
                try {
                    Bitmap bmp = null;

                    //디바이스 이미지와 매칭
                    if (localMatchingFiles.contains(src)) {
                        BitmapFactory.Options options = new BitmapFactory.Options();
                        options.inPreferredConfig = Bitmap.Config.RGB_565; //Bitmap.Config.ARGB_8888;
                        options.inDither = false;
                        bmp = BitmapFactory.decodeFile( ((AppConfig) act.getApplicationContext()).getImage_Folder() + "/"+ src, options);
                    } else {
                        InputStream ims = act.getAssets().open(src);
                        bmp = BitmapFactory.decodeStream(ims);
                    }

                    if (bmp != null) {
                        ByteArrayOutputStream stream = new ByteArrayOutputStream();

                        int iWidth = Constants.IMAGE_REDUCED;   // 축소시킬 너비
                        int iHeight = Constants.IMAGE_REDUCED;   // 축소시킬 높이
                        float fWidth = bmp.getWidth();
                        float fHeight = bmp.getHeight();

                        if (fWidth > iWidth) { // 원하는 널이보다 클 경우의 설정
                            float mWidth = (float) (fWidth / 100);
                            float fScale = (float) (iWidth / mWidth);
                            fWidth *= (fScale / 100);
                            fHeight *= (fScale / 100);

                        } else if (fHeight > iHeight) { // 원하는 높이보다 클 경우의 설정
                            float mHeight = (float) (fHeight / 100);
                            float fScale = (float) (iHeight / mHeight);
                            fWidth *= (fScale / 100);
                            fHeight *= (fScale / 100);
                        }

                        try {
                            Bitmap resizedBmp = Bitmap.createScaledBitmap(bmp, (int) fWidth, (int) fHeight, true); // 리사이즈 이미지 동일파일명 덮어 쒸우기 작업
                            resizedBmp.compress(Bitmap.CompressFormat.PNG,  Constants.QUALITY, stream);
                            img = Image.getInstance(stream.toByteArray());
                            super.store(src, img);
                            bmp.recycle();
                        } catch (Exception e) {
                            DLog.d(e.getMessage());
                        }
                    }
                }
                catch (Exception e) {
                    DLog.d(e.getMessage());
                }
            }

            sendBroadCase(i);
            i++;
            return img;
        }
    }

```

