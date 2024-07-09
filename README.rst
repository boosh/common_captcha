==============
common-captcha
==============

common-captcha is a CAPTCHA program, including simple CAPTCHA and slider CAPTCHA, based on user behavior verification.

Features
====================================

- Simple CAPTCHA
- Slider puzzle CAPTCHA

Introduction
====================================

Install with pip:

.. code-block:: console

    $ pip install common-captcha

Preview
====================================

Slider CAPTCHA

https://github.com/Asamusummer/common_captcha/blob/main/%E6%BB%91%E5%8A%A8%E6%8B%BC%E5%9B%BE.gif


Click word CAPTCHA

https://github.com/Asamusummer/common_captcha/blob/main/%E7%82%B9%E9%80%89%E6%96%87%E5%AD%97.gif


How To Use?
====================================

Before that, you need a usable Redis instance.

.. code-block:: console

    redis_url redis://xxxxxx:xxxxxx@xxxxx:xxx/11


Configuration
====================================

.. code-block:: python

    class SimpleCaptchaConfig(BaseConfig):
        """ Simple CAPTCHA configuration """

        simple_captcha_cache_key = "SimpleCaptcha"  # simple captcha redis cache key
        simple_captcha_cache_key_expire = 6000  # simple captcha redis cache key expired time


    class BlockPuzzleCaptchaConfig(BaseConfig):
        """ Slider CAPTCHA configuration """

        block_puzzle_captcha_cache_key = "BlockPuzzleCaptcha"  # block puzzle captcha redis cache key
        block_puzzle_captcha_cache_key_expire = 6000  # block puzzle captcha redis cache key expired time
        block_puzzle_captcha_check_offsetX = 10  # block puzzle captcha verify offset x
        background_image_root_path = "resource/defaultImages/jigsaw/original"  # block puzzle background images
        template_image_root_path = "resource/defaultImages/jigsaw/slidingBlock"  # block puzzle template images
        pic_click_root_path = "resource/defaultImages/pic-click"  # block puzzle pic check images
        font_ttf_root_path = "resource/fonts/WenQuanZhengHei.ttf"  # block puzzle font.ttf
        font_water_text = "lei.wang"  # block puzzle captcha water text
        font_water_text_font_size = 22  # block puzzle captcha water text font size


    class ClickWordCaptchaConfig(BaseConfig):
        """ Click word CAPTCHA configuration """

        click_word_captcha_cache_key = "ClickWordCaptcha"  # click word captcha redis cache key
        click_word_captcha_cache_key_expire = 6000  # click_word_captcha_cache_key expired time
        font_ttf_root_path = "resource/fonts/WenQuanZhengHei.ttf"  # click word captcha font.ttf
        click_word_captcha_font_number = 4
        click_word_captcha_font_size = 25
        background_image_root_path = "resource/defaultImages/jigsaw/original"  # click word captcha background images
        click_word_captcha_text = "of a the is I am not in people have come he this on with a ground to big inside say then go child get also and that want down look sky time past out small what rise you all put good still more not for again can family learn only with main will like year think live same old middle ten from self in front head road it after then walk very like see two use her country move advance become return what side make correct open and some now mountain people wait through hair work towards matter life give long water few justice three sound at master know reason eye will point heart battle two ask but body side real eat make call when live hear change hit huh true whole just four already enemy of most light produce feeling road divide total stripe plain talk east seat next close as flower mouth put child often gas five number write army bar culture transport again fruit how decide allow fast clear go because other fly outside tree thing live department none towards ship hope new lead team first strength complete but stand on behalf member machine more nine you each wind level with laugh ah child ten thousand few straight meaning night compare level link car heavy convenient fight horse which change too refer transform society similar scholar who dry stone full day decide hundred original take group study each six this think explain stand river village eight difficult early discuss what root together let mutual research now its book sit connect should believe feel step opposite place remember will thousand look fight lead or teacher conclude block run who grass exceed word add foot tight love etc practice array fear moon green half fire law question build rush place sing sea seven woman duty piece feel accurate Zhang group house leave color face section reverse eye benefit world just and from send cut star guide evening watch enough whole recognize sound snow flow not place should and bottom deep carve flat great busy lift indeed close light talk agriculture ancient black tell world pull name yeah earth clear sun shine do history change calendar turn draw make mouth this treat north must serve rain wear inside recognize test industry vegetable climb sleep rise shape amount us observe bitter body crowd through rush together break friend spend technique meal side room extreme south gun read sand year line wild firm empty receive calculate to politics city work fall money special surround younger brother win teach hot expand package song type gradually strong number village call gender sound answer brother border old god seat help okay receive series order jump not what cow take enter shore dare drop ignore kind equip top urgent forest stop breathe sentence area clothes generally report leaf press slow uncle back fine"


When you need to customize the configuration, you need to override the corresponding properties and pass them to the CAPTCHA instance

demo
======

.. code-block:: python

    from common_captcha.config import BlockPuzzleCaptchaConfig as _baseConfig

    class BlockPuzzleCaptchaConfig(_baseConfig):
        font_water_text_font_size = 30
        font_water_text = "Communication University of China"


Simple CAPTCHA:

.. code-block:: python

    from common_captcha.strategy.simple_captcha import SimpleCaptcha

    simple_captcha = SimpleCaptcha(redis_url="redis://xxxxxx:xxxxxx@xxxxx:xxx/11", configs=BlockPuzzleCaptchaConfig)
    print(simple_captcha.get())
    print(simple_captcha.verify({"token": "", "code": ""}))


Slider CAPTCHA:

.. code-block:: python

    from common_captcha.strategy.block_puzzle_captcha import BlockPuzzleCaptcha

    block_captcha = BlockPuzzleCaptcha(redis_url="redis://xxxxxx:xxxxxx@xxxxx:xxx/11")
    print(block_captcha.get())
    print(block_captcha.verify(token="", point_json={"x": "", "y": ""}))

Click word CAPTCHA:

.. code-block:: python

    from common_captcha.strategy.click_word_captcha import ClickWordCaptcha

    click_captcha = ClickWordCaptcha(redis_url="redis://xxxxxx:xxxxxx@xxxxx:xxx/11")
    token = "2a6d0134672845469904d9d541c93f60"
    point_jsons = [
        {
            "x": 17,
            "y": 187
        },
        {
            "x": 140,
            "y": 43
        },
        {
            "x": 193,
            "y": 64
        }
    ]
    print(click_captcha.verify(token, point_jsons))

