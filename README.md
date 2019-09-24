# testRepository


[YouCheatSheet](https://www.vagrantup.com/docs/provisioning/)
```
After run vagrant file:
                                                            |               SOME COMPANY :)
                                                            |
                                                            |
                                                            |
|Your host machine| ---1---- |Server in internet| ---2-- |Router in are company| ---3--- | External working server | ---4-- | Internal working server 1|
                                                      (only use for masquerade)                                         |
                                                            |                                                           |
                                                            |                                                           |
                                                            |                                                           |---| Internal working server 2|

1) public network
2) public network(but we dont know about "Router in are company" ip address, private net too)
3) private network
4) private network
```

### Your task:
1. first branch - undesterstand vagrant file and use provision from bash script file instead inline script.
2. second branch - provision on ansible with same action as on bash.
3. third branch - coming soon :)

