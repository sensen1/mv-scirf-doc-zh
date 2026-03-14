IO
==
此软件包提供用于输入/输出的函数和对象。

通过 :class:`~touchstone.Touchstone` 类，可以读取和写入 Touchstone 文件，并且可以通过网络构造函数 :func:`~skrf.network.Network.__init__` 更轻松地使用该类。

可以使用通用函数 :func:`~skrf.io.general.read` 和 :func:`~skrf.io.general.write` 将 [几乎] 任何 skrf 对象读取和写入磁盘，使用 :mod:`pickle` 模块。这仅应用于临时存储，因为 pickle 序列化在时间上不稳定，并且随着 skrf 的发展，它可能会发生变化。

.. automodule:: skrf.io.touchstone

.. automodule:: skrf.io.general

.. automodule:: skrf.io.csv