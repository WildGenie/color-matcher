=============
color-matcher
=============

Description
-----------

*color-matcher* enables color transfer across images which comes in handy for automatic color-grading
of photographs, paintings and film sequences as well as light-field and stopmotion corrections. The methods behind
the mappings are based on the approach from Reinhard *et al.*, the Monge-Kantorovich Linearization (MKL) as proposed by
Pitie *et al.* and our analytical solution to a Multi-Variate Gaussian Distribution (MVGD) transfer in conjunction with
classical histogram matching. As shown below our HM-MVGD-HM compound outperforms existing methods.

|release| |license| |build_github| |coverage| |pypi_total| |pypi|

|binder|

Results
-------

|vspace|

.. list-table::
   :widths: 8 8 8 8
   :header-rows: 1
   :stub-columns: 1

   * -
     - Source
     - Target
     - Result
   * - Photograph
     - |src_photo|
     - |ref_photo|
     - |res_photo|
   * - Film sequence
     - |src_seq|
     - |ref_seq|
     - |res_seq|
   * - Light-field correction
     - |src_lfp|
     - |ref_lfp|
     - |res_lfp|
   * - Paintings
     - |src_paint|
     - |ref_paint|
     - |res_paint|

|

Installation
------------

* via pip:
    1. install with ``pip3 install color-matcher``
    2. type ``color-matcher -h`` to the command line once installation finished

* from source:
    1. install Python from https://www.python.org/
    2. download the source_ using ``git clone https://github.com/hahnec/color-matcher.git``
    3. go to the root directory ``cd color-matcher``
    4. load dependencies ``$ pip3 install -r requirements.txt``
    5. install with ``python3 setup.py install``
    6. if installation ran smoothly, enter ``color-matcher -h`` to the command line

CLI Usage
---------

From the root directory of your downloaded repo, you can run the tool on the provided test data by

``color-matcher -s './tests/data/scotland_house.png' -r './tests/data/scotland_plain.png'``

on a UNIX system where the result is found at ``./tests/data/``. A windows equivalent of the above command is

``color-matcher --src=".\\tests\\data\\scotland_house.png" --ref=".\\tests\\data\\scotland_plain.png"``

Alternatively, you can specify the method or select your images manually with

``color-matcher --win --method='hm-mkl-hm'``

Note that batch processing is possible by passing a source directory, e.g., via

``color-matcher -s './tests/data/' -r './tests/data/scotland_plain.png'``

More information on optional arguments, can be found using the help parameter

``color-matcher -h``

API Usage
---------

.. code-block:: python

    from color_matcher import ColorMatcher
    from color_matcher.io_handler import load_img_file, save_img_file, FILE_EXTS
    from color_matcher.normalizer import Normalizer
    import os

    img_ref = load_img_file('./tests/data/scotland_plain.png')

    src_path = '.'
    filenames = [os.path.join(src_path, f) for f in os.listdir(src_path)
                         if f.lower().endswith(FILE_EXTS)]

    cm = ColorMatcher()
    for i, fname in enumerate(filenames):
        img_src = load_img_file(fname)
        img_res = cm.transfer(src=img_src, ref=img_ref, method='mkl')
        img_res = Normalizer(img_res).uint8_norm()
        save_img_file(img_res, os.path.join(os.path.dirname(fname), str(i)+'.png'))


.. Hyperlink aliases

.. _source: https://github.com/hahnec/color-matcher/archive/master.zip

.. |src_photo| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/scotland_house.png" width="200px" max-width:"100%">

.. |ref_photo| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/scotland_plain.png" width="200px" max-width:"100%">

.. |res_photo| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/scotland_pitie.png" width="200px" max-width:"100%">

.. |src_paint| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/parismusees/cezanne_paul_trois_baigneuses.png" width="200px" max-width:"100%">

.. |ref_paint| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/parismusees/cezanne_paul_portrait_dambroise_vollard.png" width="200px" max-width:"100%">

.. |res_paint| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/parismusees/cezanne_paul_trois_baigneuses_mvgd.png" width="200px" max-width:"100%">

.. |src_seq| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/wave.gif" width="200px" max-width:"100%">

.. |ref_seq| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/sunrise.png" width="200px" max-width:"100%">

.. |res_seq| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/wave_mvgd.gif" width="200px" max-width:"100%">

.. |src_lfp| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/view_animation_7px.gif" width="200px" max-width:"100%">

.. |ref_lfp| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/bee_2.png" width="200px" max-width:"100%">

.. |res_lfp| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/master/tests/data/view_animation_7px_hm-mkl-hm.gif" width="200px" max-width:"100%">

.. |vspace| raw:: latex

   \vspace{1mm}

.. |metric_chart| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/develop/docs/img/hist+wasser_dist.svg" max-width="100%" align="center">

.. |metric_latex| raw:: latex

    W_1 = \int_{0}^{\infty} \left| F\left(\mathbf{r}^{(g)}\right) - F\left(\mathbf{z}^{(g)}\right) \right|_1 \, \mathrm{d}k

    D_2 = \left\| f(\mathbf{r}) - f(\mathbf{z}) \right\|_2

.. |metric_eqs| raw:: html

    <img src="https://raw.githubusercontent.com/hahnec/color-matcher/develop/docs/img/distance_metrics.svg" max-width="100%" align="center">


.. Image substitutions

.. |release| image:: https://img.shields.io/github/v/release/hahnec/color-matcher?style=square
    :target: https://github.com/hahnec/color-matcher/releases/
    :alt: release

.. |license| image:: https://img.shields.io/badge/License-GPL%20v3.0-orange.svg?style=square
    :target: https://www.gnu.org/licenses/gpl-3.0.en.html
    :alt: License

.. |build_travis| image:: https://img.shields.io/travis/com/hahnec/color-matcher?style=square
    :target: https://travis-ci.com/github/hahnec/color-matcher

.. |build_github| image:: https://img.shields.io/github/workflow/status/hahnec/color-matcher/ColorMatcher's%20CI%20Pipeline/master?style=square
    :target: https://github.com/hahnec/color-matcher/actions
    :alt: GitHub Workflow Status

.. |coverage| image:: https://img.shields.io/coveralls/github/hahnec/color-matcher?style=square
    :target: https://coveralls.io/github/hahnec/color-matcher

.. |pypi| image:: https://img.shields.io/pypi/dm/color-matcher?label=PyPI%20downloads&style=square
    :target: https://pypi.org/project/color-matcher/
    :alt: PyPI Downloads

.. |pypi_total| image:: https://pepy.tech/badge/color-matcher?style=flat-square
    :target: https://pepy.tech/project/color-matcher
    :alt: PyPi Dl2

.. |binder| image:: https://img.shields.io/badge/launch-binder-579aca.svg?logo=data:image/png;base64,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
   :target: https://gesis.mybinder.org/binder/v2/gh/hahnec/color-matcher/3a85a06bb546fa6d4294c7fc241de9e3cce2b2e0?urlpath=lab%2Ftree%2F01_api_demo.ipynb

.. |paper| image:: http://img.shields.io/badge/paper-arxiv.2010.11687-red.svg?style=flat-square
    :target: https://arxiv.org/pdf/2010.11687.pdf
    :alt: arXiv link

Experimental results
--------------------

|metric_chart|

The above diagram illustrates light-field color consistency from Wasserstein metric :math:`W_1` and histogram distance
:math:`D_2` where low values indicate higher similarity between source :math:`\mathbf{r}` and target :math:`\mathbf{z}`.
These distance metrics are computed as follows

|metric_eqs|

where :math:`f(k,\cdot)` and :math:`F(k,\cdot)` represent the Probability Density Function (PDF) and Cumulative Density Function (CDF) at intensity level :math:`k`, respectively.
More detailed information can be found in `our IEEE paper <https://arxiv.org/pdf/2010.11687.pdf>`__.

|vspace|

Citation
--------

.. code-block:: BibTeX

    @ARTICLE{plenopticam,
        author={Hahne, Christopher and Aggoun, Amar},
        journal={IEEE Transactions on Image Processing},
        title={PlenoptiCam v1.0: A Light-Field Imaging Framework},
        year={2021},
        volume={30},
        number={},
        pages={6757-6771},
        doi={10.1109/TIP.2021.3095671}
    }

Author
------

`Christopher Hahne <http://www.christopherhahne.de/>`__