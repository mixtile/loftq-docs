# scons for rest

env = Environment(tools=['rest'])

env['REST_PDFOPTION'] = '--use-floating-images --use-numbered-links --section-header-depth=2'

html_stylesheet = "./_static/flask.css"
en_pdfstylesheet = "./_static/en-US.style"
zh_pdfstylesheet = './_static/zh-CN.style' 

en_docs = [
'en/en-android-cust.rst',
'en/en-intro.rst',
'en/en-legacy-quick-guide.rst',
'en/en-mainline-quick-guide.rst',
'en/en-owncloud-howto.rst',
'en/en-seafile-howto.rst',
]

zh_docs = [
'zh/zh-intro.rst',
'zh/zh-product-intro.rst',
'zh/zh-android-network-debug.rst',
'zh/zh-jn5168-guide.rst',
'zh/zh-legacy-driver-guide.rst',
'zh/zh-legacy-quick-guide.rst',
'zh/zh-mainline-quick-guide.rst',
'zh/zh-owncloud-howto.rst',
'zh/zh-seafile-howto.rst',
]

#env.Rst2Html(en_docs, stylesheet=html_stylesheet)
env.Rst2PDF(en_docs, stylesheet=en_pdfstylesheet)

#env.Rst2Html(zh_docs, stylesheet=html_stylesheet)
env.Rst2PDF(zh_docs, stylesheet=zh_pdfstylesheet)
