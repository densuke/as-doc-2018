.. _vagrant:

==============================
vagrantの基本操作
==============================

たしかソフトⅡコースの際にやってると思うので、確認・復習が含まれています。

Vagrantプロジェクトの作成
============================

- Vagrantのディレクトリを作り、入る
- :command:`vagrant init` で :file:`Vagrantfile` を作成します
- 必要に応じて :file:`Vagrantfile` を編集します
- :command:`vagrant up` などでVMを操作します

box
------

- VagrantではVMイメージを抽象化して **box** と呼んでいる
- boxは自分で作ることもできるし、出来合いのものを持ってくることもできる
- とりあえずのものは外から持ち込み、カスタマイズしたものを手元で持っておくと使い回ししやすい
- 外から持ってくる場合、よく使うのは **Vagrant cloud** のサービス

(実習)Vagrant cloudからの持ち込み〜起動
-------------------------------------------------

1. 基本のboxとして :code:`ubuntu/xenial64` を持ち込んでみる
2. プロジェクトディレクトリとして、 :file:`~/as/r-mybox` を作成してその中で作業するものとする


.. code-block:: bash
    :caption: 基本のプロジェクト作成

    $ mkdir -pv ~/as/r-mybox
    $ cd ~/as/r-mybox
    $ vagrant init ubuntu/xenial64

.. note::

    :command:`vagrant` に渡しているboxの名前を間違うことはよくあります。
    そういうときは、 :file:`Vagrantfile` をエディタで開いて、
    直接書き換えるといいでしょう。

    .. code-block:: bash
    
        $ vagrant init ubunutu/xenial64
        #              ^^^^^^^
        #                スペルミスしてしまった

    おもむろにエディタで :file:`Vagrantfile` を開きます。

    .. code-block:: ruby
        :linenos:
        :lineno-start: 8
        :emphasize-lines: 8

        Vagrant.configure("2") do |config|
            # The most common configuration options are documented and commented below.
            # For a complete reference, please see the online documentation at
            # https://docs.vagrantup.com.

            # Every Vagrant development environment requires a box. You can search for
            # boxes at https://vagrantcloud.com/search.
            config.vm.box = "ubunutu/xenial64"

            # Disable automatic box update checking. If you disable this, then
            # boxes will only be checked for updates when the user runs
            # `vagrant box outdated`. This is not recommended.
            # config.vm.box_check_update = false
    
    見ての通り、15行目に間違ったbox名があるので、修正すればOKです。
    このテクニックは、 :command:`vagrant init` の時にうっかりboxを渡しそこねた時にも使えます。

起動、終了、再起動
=================================

- VMの起動などの処理ももちろん :command:`vagrant` にて行います
- 起動は :command:`up`
- 終了は :command:`halt` ※ :command:`shutdown` や :command:`down` ではない
- 再起動は :command:`reload` ※ haltしてupするだけ
- 現状のチェックには :command:`status`

(実習)VMの起動処理
-----------------------------

.. code-block:: bash
    :emphasize-lines: 5-

    $ vagrant up
    ... 初回起動では必要に応じてboxのダウンロードが行われます
    (その後起動処理)
    $ vagrant status
    Current machine states:

    default                   running (virtualbox)

    The VM is running. To stop this VM, you can run `vagrant halt` to
    shut it down forcefully, or you can run `vagrant suspend` to simply
    suspend the virtual machine. In either case, to restart it again,
    simply run `vagrant up`.

- 同じ手順で、 :command:`halt` や :command:`reload` を実行してみましょう
- :command:`status` も試してください。