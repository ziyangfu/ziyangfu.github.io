### wireshark识别解析SOMEIP协议

显示效果如下：

![wireshark解析someip协议](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/wireshark%E8%AF%86%E5%88%AB%E8%A7%A3%E6%9E%90SOMEIP%E5%8D%8F%E8%AE%AE%E7%A4%BA%E4%BE%8B.webp)

wireshark识别应用层协议采用 端口号-应用层协议绑定（例如：port 80 = HTTP）及自启发识别机制，而SOMEIP协议 wireshark无法通过端口号识别，自启发识别算法失效，为了解析SOMEIP协议分组，手动设置如下：

step1：

![wireshark解析someip协议步骤1](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/wireshark%E8%A7%A3%E6%9E%90someip%E5%8D%8F%E8%AE%AE%E6%AD%A5%E9%AA%A41.webp)

step2：选择SOMEIP协议

![步骤2](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/wireshark%E8%A7%A3%E6%9E%90someip%E5%8D%8F%E8%AE%AE%E6%AD%A5%E9%AA%A42.webp)

保持永久设置，实质是绑定端口与协议

点击菜单栏->edit->preference->protocol,选择SOMEIP协议

![步骤3](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/wireshark%E8%A7%A3%E6%9E%90someip%E5%8D%8F%E8%AE%AE%E6%AD%A5%E9%AA%A42.webp)

SOMEIP 及SOMEIP-SD 解析

![someip_01](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/wireshark%E8%A7%A3%E6%9E%90someip%E5%8D%8F%E8%AE%AE%E6%AD%A5%E9%AA%A42.webp)

![someip_02](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/someip_02.png)

![someip_03](https://fzy-1313034652.cos.ap-shanghai.myqcloud.com/%E8%BD%A6%E8%BD%BD%E4%BB%A5%E5%A4%AA%E7%BD%91/someip_03.png)