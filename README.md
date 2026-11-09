# SimpleTCP

网络课程设计（C）：在 UDP 之上实现一个简化的 TCP 协议 SimpTCP，并以与 BSD socket 同名的接口提供给应用程序。

## 架构

- **协议实体**：全局的 `simptcp_entity` 保存与具体 socket 无关的状态；每个 socket 按当前状态调用状态机里对应的处理函数（`simptcp_socket_states_funcs`），实现连接建立、数据传输、确认与关闭。
- **传输**：SimpTCP 的 PDU 作为 UDP 报文的载荷收发，底层 UDP socket 设为非阻塞；收到的 PDU 按远端地址分发到对应的 SimpTCP socket（`demultiplex_packet`）。
- **接口**：`simptcp_api` 提供 `socket`、`bind`、`listen`、`accept`、`connect`、`send`、`recv`、`read`、`write`、`shutdown`、`close`，应用程序无需改动即可使用。

| 文件 | 作用 |
|---|---|
| `inc/`、`src/simptcp_api.*` | socket 接口 |
| `src/simptcp_entity.*` | 协议实体与报文分发 |
| `src/simptcp_packet.*` | PDU 的构造与解析 |
| `src/simptcp_lib.*`、`src/simptcp_func_var.c` | 各状态的处理函数与工具函数 |
| `src/client.c`、`src/server.c` | 测试用的客户端与服务端 |
| `doc/` | Doxygen 文档 |

## 构建

```bash
cd stcp14-*/
make
```
