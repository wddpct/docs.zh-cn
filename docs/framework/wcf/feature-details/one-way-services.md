---
title: "单向服务 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "服务协定 [WCF], 定义单向"
  - "WCF [WCF], 单向服务约定"
  - "Windows Communication Foundation [WCF], 单向服务约定"
ms.assetid: 19053a36-4492-45a3-bfe6-0365ee0205a3
caps.latest.revision: 18
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 18
---
# 单向服务
服务操作的默认行为是请求\-答复模式。在请求\-答复模式中，即使服务操作以代码形式表示为 `void` 方法，客户端也会等待答复消息。使用单向操作时，只能传输一个消息。接收方不发送答复消息，发送方也不需要获得答复消息。  
  
 可以在以下情况下使用单向设计模式：  
  
-   客户端必须调用操作且在操作级别不受操作结果的影响。  
  
-   使用 <xref:System.ServiceModel.NetMsmqBinding> 或 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 类。（[!INCLUDE[crabout](../../../../includes/crabout-md.md)]此方案的更多信息，请参见 [WCF 中的队列](../../../../docs/framework/wcf/feature-details/queues-in-wcf.md)。）  
  
 如果是单向操作，则不会向客户端返回承载错误信息的响应消息。可以通过使用基础绑定的功能（如可靠会话）或通过设计一个可使用两个单向操作的双工服务协定（一个单向协定从客户端到服务，用于调用服务操作；另一个单向协定在服务和客户端之间，以使服务可以使用客户端实现的回调将错误发回到客户端）来检测错误条件。  
  
 若要创建单向服务协定，请定义服务协定，将 <xref:System.ServiceModel.OperationContractAttribute> 类应用到每个操作，并将 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 属性设置为 `true`，如下面的代码示例所示。  
  
```  
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOneWayCalculator  
{  
    [OperationContract(IsOneWay=true)]  
    void Add(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Subtract(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Multiply(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Divide(double n1, double n2);  
}  
```  
  
 有关完整的示例，请参见[单向](../../../../docs/framework/wcf/samples/one-way.md)示例。  
  
## 单向操作的客户端阻止  
 值得注意的是，虽然一些单向应用程序可以在出站数据写入到网络连接后立即返回，但在很多情况下，绑定或服务的实现可导致 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端阻止使用单向操作。在 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端应用程序中，在将出站数据写入到网络连接之前，[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 客户端对象不会返回。所有消息交换模式都是如此，包括单向操作；这意味着在将数据写入传输时发生的任何问题都会阻止客户端返回。结果可能是在将消息发送到服务的过程中出现异常或延迟，具体取决于所发生的问题。  
  
 例如，如果传输找不到终结点，则会在短时间内引发 <xref:System.ServiceModel.EndpointNotFoundException?displayProperty=fullName> 异常。但是，由于某种原因，服务也可能无法读取网络上的数据，这将阻止客户端传输发送操作返回。在这些情况下，如果超出了客户端传输绑定上的 <xref:System.ServiceModel.Channels.Binding.SendTimeout%2A?displayProperty=fullName> 期限，则会引发 <xref:System.TimeoutException?displayProperty=fullName>，但会在超出超时期限后引发。也有可能向某一服务发送了过多的消息，而当超过某一特定点后，该服务无法处理这些消息。在这种情况下，单向客户端也会发生阻止，直到服务可以处理这些消息或直到引发异常。  
  
 另一种不同的情况是服务的 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A?displayProperty=fullName> 属性设置为 <xref:System.ServiceModel.ConcurrencyMode> 且绑定使用会话。在这种情况下，调度程序会对传入消息强制执行排序（会话的要求），这会阻止从网络上读取后续的消息，直到服务处理完该会话的前一个消息为止。同样，客户端会发生阻止，但是否发生异常取决于客户端的超时设置结束前，服务是否能处理等待的数据。  
  
 通过在客户端对象和客户端传输发送操作之间插入一个缓冲器可以使此问题得到一些缓解。例如，使用异步调用或使用内存中的消息队列可以使客户端对象迅速返回。这两种方法都可以增强功能，但线程池和消息队列的大小仍具有限制。  
  
 建议改为检查服务以及客户端上的各个控制机制，然后测试应用程序方案来确定任一端最佳配置。例如，如果使用会话会在服务上阻止消息的处理，则可以将 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A?displayProperty=fullName> 属性设置为 <xref:System.ServiceModel.InstanceContextMode>，使每个消息都可以通过不同的服务实例来处理，并将 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 设置为 <xref:System.ServiceModel.ConcurrencyMode>，以便允许多个线程一次调度多个消息。另一个方法是提高服务和客户端绑定的读取配额。  
  
## 请参阅  
 [单向](../../../../docs/framework/wcf/samples/one-way.md)