fuchsia::modular::Agent
=====

---
[*英文原文快照*](https://github.com/fuchsia-mirror/peridot/blob/4fda54363e0bc82f253778a2291016b4fbae120f/docs/modular/agent.md)

---

<!-- An agent is an application that implements the fuchsia::modular::Agent interface, whose lifecycle
is not tied to any Story and is a singleton in User scope. An agent can be
invoked by other components or by the system in response to triggers. An agent
can terminate itself or be terminated by the system. An agent can provide /
receive services to / from other applications, send / receive messages and give
suggestions to the user. -->

Agent是一个实现`fuchsia::modular::Agent`接口的应用程序，其生命周期不依赖于任何Story，并且是User作用域内的单例。Agent可以被其他组件或系统调用以响应触发器，并且Agent可以自行终止或由系统终止。Agent可以向/从其他应用程序提供/接收服务，发送/接收消息并向用户提供建议。

<!-- ## See also: -->
## 请查看

[fuchsia::modular::Agent](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/agent/agent.fidl)

[fuchsia::modular::AgentContext](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/agent/agent_context.fidl)

[fuchsia::modular::AgentController](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/agent/agent_controller/agent_controller.fidl)