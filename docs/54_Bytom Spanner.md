---
id: docs_54
title: Bytom Spanner
sidebar_label: Bytom Spanner
---

##### btm-spanner

基于python的比原工具
要求: Python 3.x,和相关需要的应用包
启动依赖包:

    pip install requests

初始化:

    bytomd init --chain_id mainnet

如果你不知道怎么启动运行比原全节点钱包，请参考：[wiki](https://github.com/Bytom/bytom/wiki/Build-and-Install)

##### btm-sender

btm-sender是一款批量发送BTM到大量地址的转账工具
用法:

btmspanner.py btmsender [-h] -i I -a A -p P [-c C]

参数:

    -h help         --show this help message and exit
    -i input        --transaction txt file
    -a account      --wallet account id
    -p password     --wallet account password
    -c count        --transaction output count
    -u use          --unconfirmed UTXO build transaction
    -t time_range   --the transaction will not be submitted into block after this height

想了解btm-sender工具更多详细信息，阅读：[README.md](https://github.com/Bytom/btm-spanner/blob/master/btmsender/README.md) 

##### utxo-merger

比原链合并UTXO的工具
用法:

    $ python btmspanner.py utxomerger -h
      usage: btmspanner.py [-h] [-o URL] [-a ACCOUNT_ALIAS] [-p PASSWORD] [-x MAX_AMOUNT] [-s MIN_AMOUNT] [-l] [-m MERGE_LIST] [-f FOR_LOOP] [-y]
           
Bytom merge utxo tool

    optional arguments:
           -h help              --show this help message and exit
           -o url URL           --API url to connect
           -a ACCOUNT_ALIAS     --account ACCOUNT_ALIAS account alias
           -p PASSWORD          --pass PASSWORD account password
           -x MAX_AMOUNT        --max MAX_AMOUNT range lower than max_amount
           -s MIN_AMOUNT        --min MIN_AMOUNT range higher than min_amount
           -l list              --Show UTXO list without merge
           -m MERGE_LIST        --merge MERGE_LIST UTXO to merge
           -f FOR_LOOP          --forloop FOR_LOOP size for loop of UTXO to merge
           -y yes               --confirm transfer

了解更多合并UTXO工具的详细信息参考: [README.md](https://github.com/Bytom/btm-spanner/blob/master/utxomerger/README.md) 
