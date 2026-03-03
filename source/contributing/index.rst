.. _contributing:
    :github_url:


.. image:: ../_static/scikit-rf-title-flat.svg
    :target: ../_static/scikit-rf-title-flat.svg
    :height: 100
    :align: center


为 scikit-rf 做贡献
==========================

赞助项目
----------------------
可以通过 GitHub 赞助选项（"赞助此项目"）来赞助 scikit-rf 软件包的维护者和开发者，该选项可在 `scikit-rf GitHub 页面 <https://github.com/scikit-rf/scikit-rf>`_ 上看到。

赞助是为开源做贡献的另一种方式：在经济上支持构建和维护它的人。资助个人有助于他们继续从事重要工作，扩大参与机会，并给予开发者应有的认可。


贡献代码
------------------------

.. note:: 如果您觉得本页面的说明太复杂，但您仍然想贡献，请随时通过 `scikit-rf 邮件列表 <https://groups.google.com/forum/#!forum/scikit-rf>`_ 给我们发邮件，或加入我们的 `scikit-rf Slack 频道 <https://join.slack.com/t/scikit-rf/shared_invite/zt-d82b62wg-0bdSJjZVhHBKf6687V80Jg>`_ 或 `scikit-rf Matrix/Element 频道 <https://app.element.io/#/room/#scikit-rf:matrix.org>`_ 寻求帮助。


**skrf** 使用 "Fork + Pull" 协作开发模式。如果这对您来说是新的，请参阅 GitHub 关于 `forking <https://help.github.com/articles/fork-a-repo>`_ 和 `pull requests <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests>`_ 的文章。

基本上，您将：

1. `Fork <https://help.github.com/articles/fork-a-repo>`_ `GitHub scikit-rf 仓库 <https://github.com/scikit-rf/scikit-rf>`_,

2. 进行您的修改。

3. 然后发送一个 `pull request (PR) <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests>`_，以便将您的添加内容合并到主 scikit-rf 仓库中。您的提案将被审查或讨论，您可能会收到一些评论，这些评论只是为了使您的贡献尽可能完善！


.. tip:: 在本地进行修改时，您可能需要在一个专门的开发环境中工作，以免干扰您 `已经安装 <../tutorials/Installation.html>`_ 的 scikit-rf 软件包。例如，您可以使用 `anaconda 环境 <https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>`_。为了让 anaconda 环境找到您的本地 scikit-rf 仓库，请使用方便的 `conda-develop <https://docs.conda.io/projects/conda-build/en/latest/resources/commands/conda-develop.html>`_ 命令。

Prek
++++

您可以使用 prek_ 来自动化 **scikit-rf** 中的代码质量检查并执行自动修复。

要在您的计算机上启用 prek（首先确保已安装），打开终端并导航到包含 scikit-rf 仓库克隆的 :file:`scikit-rf/` 目录，然后运行：

.. code-block:: bash

    prek install

.. note::

   一旦为仓库安装了 prek，每次您尝试提交更改时 prek 都会运行。如果任何 prek 检查失败，或者 prek 更改了任何文件，则需要对更改的文件重新执行 `git add` 并再次 `git commit`。

.. tip::

   要在不运行 prek 的情况下提交更改，请使用 git 的 `-n` 或 `--no-verify` 标志。


基本 git 命令行工作流
+++++++++++++++++++++++++++++++

以下是可以用于贡献代码的 git 命令的基本示例。

.. code-block:: sh

    # 在 GitHub 界面中创建您自己的 scikit-rf fork，比如 ME/scikit-rf

    # 使用以下任一方式本地克隆您的新 fork：
    git clone ME@github.com:ME/scikit-rf.git

    # 如果您已设置 ssh 密钥，如果没有：
    git clone https://github.com/scikit-rf/scikit-rf.git

    # ...进行您的更改...

    # 本地提交更改
    git commit -a

    # 将更改推送到您的仓库
    git push origin

    # 在 github.com 上创建 pull request


保持同步
++++++++++++++++++++

要使您的本地版本与 scikit-rf 仓库保持同步（最新），`添加一个 "remote"（称之为 "upstream"） <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork>`_。从这个 remote，您可以 `"fetch" 和 "merge" 官方 scikit-rf 仓库的更改到您的本地仓库 <https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork>`_.

.. code-block:: sh

    git remote add upstream https://github.com/scikit-rf/scikit-rf.git

    # 从原始仓库获取任何新更改
    git fetch upstream

    # 将获取的任何更改合并到您的工作文件中
    git merge upstream/master



测试
+++++

测试对于软件的可靠性和可维护性至关重要。编写测试通常需要额外的努力，但从长远来看可以节省时间。测试使我们能够快速发现何时引入了新错误。这也是提供函数和类最初预期使用方式的示例的一种方式。

在提交 Pull Request 之前，我们建议贡献者在本地运行测试，以检查在其修改后是否没有任何东西被破坏。此外，我们强烈建议在添加新功能时提供新的测试。

测试结构通常遵循 `numpy/scipy <https://github.com/numpy/numpy/blob/master/doc/TESTS.rst>`_ 的惯例。测试用例位于它们正在测试的模块或子模块中，并位于名为 `tests` 的目录中。因此，media 模块的测试位于 `skrf/media/tests/`。
测试可以最容易地使用 `pytest <https://docs.pytest.org/en/latest/index.html>`_ 运行。

您可能 **不想** 将其余测试与虚拟仪器 ``skrf.vi`` 的测试一起运行，因此默认情况下排除这些测试。

要运行所有测试（除了虚拟仪器）

.. code-block:: sh

    cd scikit-rf
    # 创建环境并安装依赖项，只需执行一次。
    python -m venv .venv
    pip install -e .[test,visa,netw,xlsx,plot,docs,testspice] --compile

    # 激活环境，以下所有步骤都需要。
    # 在 Linux 系统上
    . .venv/bin/activate

    # 在 Windows 上
    .\.venv\Scripts\activate

    pytest

要在当前环境中运行所有测试 *以及* 所有教程和示例 notebooks（在提交 pull request 之前推荐）：

.. code-block:: sh

    pytest --nbval-lax


如果您想测试单个文件或目录，您需要覆盖默认的 pytest 配置并指示测试路径。例如，要仅运行与 Network 对象关联的测试（-v 以增加详细程度）：

.. code-block:: sh

    pytest -v -c "" skrf/tests/test_network.py


也可以使用正则表达式选项 (-k) 选择某些特定测试：

.. code-block:: sh

    pytest -v -c "" skrf/calibration/tests/test_calibration.py -k "test_error_ntwk"




贡献文档
----------------------------------

示例和教程
++++++++++++++++++++++

欢迎使用 scikit-rf 的使用示例，特别是在添加新功能时。我们使用 `Jupyter Notebooks <https://jupyter.org/>`_ 来编写示例和教程，它们位于 ``scikit-rf/docs/source/examples/`` 和 ``doc/source/examples`` 目录中。然后使用名为 `nbsphinx <http://nbsphinx.readthedocs.io/>`_ 的 sphinx 扩展将这些 notebooks 转换为网页。

当 Pull Request 被接受时，文档会自动构建并由 `readthedocs 提供服务 <https://scikit-rf.readthedocs.io/en/latest/>`_。构建文档所需的 Python 软件包要求保存在 ``scikit-rf/pyproject.toml`` 中。

.. important:: 在推送到您的仓库并发出 pull request 之前，至少您需要使用 notebook 中的 "Clear All Output" 命令清除 notebook 输出（或安装 `nbstripout <https://pypi.python.org/pypi/nbstripout>`_ 以便输出不被 git 跟踪（否则仓库大小将无限增长）。


参考（API）或静态文档
+++++++++++++++++++++++++++++++++++++++

文档源文件可以在 ``doc/source/`` 中找到。

函数、类和子模块的参考文档按照 `Numpy/Scipy 文档字符串格式 <https://numpydoc.readthedocs.io/en/latest/format.html>`_ 的约定记录在文档字符串中。文档整体使用 sphinx 生成，并使用 reStructed (.rst) 文本编写。

.. tip:: 如果您想自己编写一些 .rst 文件，请使用 RST 格式编辑器和检查器（例如：`<https://livesphinx.herokuapp.com/>`_），因为 Sphinx 对语法（非常）挑剔...


在本地构建文档
++++++++++++++++++++++++++++++++++

在发出关于文档的 pull request 之前，最好在本地测试您的更改是否导致所需的 HTML 输出（有时在转换为 HTML 期间可能会出现一些问题）。文档通过以下命令构建：

.. code-block:: sh

    # 确保在 scikit-rf/doc 目录中
    make html


构建的文档然后位于 ``doc/build/html`` 中。




加入 **scikit-rf** 团队！
----------------------------

您喜欢使用 scikit-rf 吗？`有商品可供您表达您的喜爱 <https://scikit-rf.org/merch.html>`_.

.. image:: https://raw.githubusercontent.com/scikit-rf/scikit-rf/master/logo/skrfshirtwhite.png
    :height: 400
    :align: center
