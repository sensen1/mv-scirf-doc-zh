参与 scikit-rf 的开发
==========================

赞助该项目
----------------------
您可以使用 GitHub 赞助功能（在 `scikit-rf GitHub 页面 <https://github.com/scikit-rf/scikit-rf>`_ 上可见，选项为“赞助此项目”）来赞助 scikit-rf 包的维护者和开发者。

赞助是另一种为开源项目做出贡献的方式：通过经济支持那些构建和维护项目的人。资助个人可以帮助他们继续完成重要的工作，扩大参与机会，并给予开发者应有的认可。

参与代码贡献
------------------------
.. 注意：如果您觉得本页面的说明过于复杂，但您仍然希望参与贡献，请不要犹豫，通过 `scikit-rf 邮件列表 <https://groups.google.com/forum/#!forum/scikit-rf>`_ 给我们发送电子邮件，或加入 `scikit-rf Slack 频道 <https://join.slack.com/t/scikit-rf/shared_invite/zt-d82b62wg-0bdSJjZVhHBKf6687V80Jg>`_ 或 `scikit-rf Matrix/Element 频道 <https://app.element.io/#/room/#scikit-rf:matrix.org>`_，以便获得帮助。

**skrf** 采用“Fork + Pull”的协作开发模式。如果您不熟悉这种模式，请查看 GitHub 上的相关文章，了解关于 `fork <https://help.github.com/articles/fork-a-repo>`_ 和 `pull requests <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests>`_ 的内容。

基本上，您需要执行以下步骤：

1. `Fork <https://help.github.com/articles/fork-a-repo>`_ `GitHub scikit-rf 仓库 <https://github.com/scikit-rf/scikit-rf>`_，

2. 进行您的修改。

3. 然后提交一个 `pull request (PR) <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests>`_，以便将您的修改合并到主 scikit-rf 仓库中。您的提案将会被审查或讨论，您可能会收到一些评论，这些评论的目的是尽可能地提高您的贡献质量！

.. 提示：在本地进行修改时，您可能需要在独立的开发环境中工作，以避免干扰您已经安装的 scikit-rf 包 `<../tutorials/Installation.html>`_。例如，您可以使用 `anaconda 环境 <https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>`_。为了使 anaconda 环境能够找到您本地的 scikit-rf 仓库，请使用便捷的 `conda-develop <https://docs.conda.io/projects/conda-build/en/latest/resources/commands/conda-develop.html>`_ 命令。

Prek
++++

您可以使用 prek 来自动化您的代码质量检查，并在 **scikit-rf** 中执行自动修复。

要在您的计算机上启用 prek（请确保已安装），请打开终端并导航到包含您 scikit-rf 仓库克隆的 `:file:`scikit-rf/` 目录，然后运行：

.. code-block:: bash

    prek install

.. 注意：

   为某个仓库安装了 prek 后，prek 将会在您尝试提交更改时运行。如果任何 prek 检查失败，或者如果 prek 修改了任何文件，则需要重新对更改的文件执行 `git add`，然后再次执行 `git commit`。

.. 提示：

   要提交更改而不运行 prek，请使用 `-n` 或 `--no-verify` 标志与 git 命令一起使用。

基本的 Git 命令行工作流程
+++++++++++++++++++++++++++++++
以下是一个基本的示例，展示了可用于参与代码贡献的 Git 命令。

.. code-block:: sh

    # 在 GitHub 界面中创建您自己的 scikit-rf 代码仓库的副本，例如 ME/scikit-rf

    # 使用以下命令克隆您新的本地代码仓库：
    git clone ME@github.com:ME/scikit-rf.git

    # 如果您已设置 SSH 密钥，或者如果没有：
    git clone https://github.com/scikit-rf/scikit-rf.git

    # ... 进行您的修改 ...

    # 本地提交更改
    git commit -a

    # 将更改推送到您的代码仓库
    git push origin

    # 在 github.com 上创建一个拉取请求。

保持同步
++++++++++++
为了使您的本地版本与 scikit-rf 仓库保持同步（更新），请添加一个“远程”仓库（命名为“upstream”）。您可以从该远程仓库“获取”并“合并”官方 scikit-rf 仓库的更改到您的本地仓库。

.. code-block:: sh

    git remote add upstream https://github.com/scikit-rf/scikit-rf.git

    # 从原始仓库获取任何新的更改
    git fetch upstream

    # 将获取的任何更改合并到您的工作文件中
    git merge upstream/master

测试
++++

测试对于软件的可靠性和可维护性至关重要。编写测试通常需要付出额外的努力，但从长远来看可以节省时间。测试使我们能够快速发现新错误。它也是提供函数和类最初的预期使用方式示例的一种方式。

在提交 Pull Request 之前，我们建议贡献者在本地运行测试，以检查他们的修改是否导致任何问题。此外，我们强烈建议在添加新功能时提供新的测试。

测试的结构通常遵循 `numpy/scipy <https://github.com/numpy/numpy/blob/master/doc/TESTS.rst>`_ 的约定。测试用例位于它所测试的模块或子模块中，并位于名为 `tests` 的目录中。因此，媒体模块的测试位于 `skrf/media/tests/`。

可以使用 `pytest <https://docs.pytest.org/en/latest/index.html>`_ 最方便地运行测试。

您可能**不**希望将虚拟仪器 `skrf.vi` 的测试与其他测试一起运行，因此默认情况下会排除这些测试。

要运行所有测试（不包括虚拟仪器）：

.. code-block:: sh

    cd scikit-rf
    # 创建环境并安装依赖项，只需执行一次。
    python -m venv .venv
    pip install -e .[test,visa,netw,xlsx,plot,docs,testspice] --compile

    # 激活环境，所有后续步骤都需要。
    # 在 Linux 系统上
    . .venv/bin/activate

    # 在 Windows 上
    .\.venv\Scripts\activate

    pytest

要运行所有测试*以及*当前环境中的所有教程和示例笔记本（在提交 Pull Request 之前建议执行）：

.. code-block:: sh

    pytest --nbval-lax

如果您想测试单个文件或目录，您需要覆盖默认的 pytest 配置并指示测试路径。例如，要仅运行与 Network 对象关联的测试（-v 选项可提高详细程度）：

.. code-block:: sh

    pytest -v -c "" skrf/tests/test_network.py

也可以使用正则表达式选项 (-k) 选择特定的测试：

.. code-block:: sh

    pytest -v -c "" skrf/calibration/tests/test_calibration.py -k "test_error_ntwk"

为文档做出贡献
--------------------

示例和教程
++++++++++++++
欢迎提供 scikit-rf 的使用示例，尤其是在添加新功能时。我们使用 `Jupyter Notebooks <https://jupyter.org/>`_ 来编写示例和教程，这些文件位于 ``scikit-rf/docs/source/examples/`` 和 ``doc/source/examples`` 目录中。然后，这些笔记本文件将被转换为带有 sphinx 扩展 `nbsphinx <http://nbsphinx.readthedocs.io/>`_ 的网页。

当 Pull Request 被接受时，文档将自动构建并由 `readthedocs <https://scikit-rf.readthedocs.io/en/latest/>`_ 提供服务。用于构建文档的 Python 包要求保存在 ``scikit-rf/pyproject.toml`` 中。

.. important:: 在推送到您的仓库并提交 Pull Request 之前，至少需要清除笔记本输出，可以使用笔记本中的“清除所有输出”命令（或安装 `nbstripout <https://pypi.python.org/pypi/nbstripout>`_，以便输出不会被 git 跟踪（否则仓库大小会无限增长）。

参考（API）或静态文档
+++++++++++++++++++++++++++++++

文档源文件可以在 ``doc/source/`` 中找到。

函数、类和子模块的参考文档记录在文档字符串中，遵循 `Numpy/Scipy 文档字符串格式 <https://numpydoc.readthedocs.io/en/latest/format.html>`_ 中规定的约定。整个文档使用 sphinx 生成，并使用 reStructured (.rst) 文本编写。

.. tip:: 如果您想自己编写一些 .rst 文件，请使用 RST 格式编辑器和检查器（例如：`<https://livesphinx.herokuapp.com/>`_），因为 Sphinx 对语法要求非常严格……

本地构建文档
++++++++++++++

在提交有关文档的 Pull Request 之前，最好在本地测试您的更改是否会生成所需的 HTML 输出（有时在转换为 HTML 期间可能会出现一些问题）。文档通过以下命令构建：

.. code-block:: sh

    # 确保您位于 scikit-rf/doc 目录中
    make html

构建后的文档位于 ``doc/build/html`` 中。

加入 **scikit-rf** 团队！
----------------------------
您喜欢使用 scikit-rf 吗？您可以通过购买周边商品来表达您的喜爱：`<https://scikit-rf.org/merch.html>`。

.. image:: https://raw.githubusercontent.com/scikit-rf/scikit-rf/master/logo/skrfshirtwhite.png
    :height: 400
    :align: center