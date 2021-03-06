# PCRF
---
## 1 概述
3GPP从R7开始在网络中引入PCRF网元结构，该功能可以对用户和业务态QoS服务质量进行控制，为用户提供差异化的服务。并且能为用户提供业务流承载资源保障以及流计费策略，真正让运营商实现基于业务和用户分类的更精细化的业务控制和计费方式，以合理利用网络资源，创造最大利润，为PS域开展多媒体实时业务提供了可靠的保障。PCRF包含策略控制决策和基于流的计费控制功能。PCRF接受来自PCC架构中的其他设备的输入，结合自身的自定义信息做出PCC决策。PCRF包括策略控制和基于流的计费控制。这两个功能各自继承了R6逻辑实体PDF和CRF。PCRF收到来自AF的会话和媒体的相关信息并通知AF。PCRF是服务数据流和IP承载资源的策略与计费控制策略点，它为PCEF策略控制与计费执行功能,选择及提供可用的策略和计费控制策略。实现业务数据流检测、门限控制、QoS控制以及基于流的计费（信用管理除外）。

####引入意义
引入PCRF后，在网络中就可实施**QoS保障技术**，这样运营商也就可以在网络中提供用户差分服务和业务的差异化服务。这样，利用PCRF节点的控制功能，可以为运营商带来如下几点好处：

- 运营商可以通过GGSN与PCRF的互控对可实现对用户的每个业务的带宽进行限制。这样可以防止网络中的资源滥用。其中一个典型情形是特定应用可能会消耗大量的可用网络资源。针对同一用户在网络中使用不同应用，运营商能够基于PCRF对每个IP流或每个应用限制用户话务。

- 运营商可以通过PS数据域的QoS保障机制对以对用户和业务进行精细的差分化管理和控制。这对开展PS数据业务具有重要的影响。

- 运营商可以通过对不同的用户等级（如预付费用户）进行用户服务区分，进而为用户提供差异化服务。例如当用户的业务使用流量达到一定的门限时，进行降低QoS 服务质量等控制。

- 运营商对于少量用户进行QoS 控制，从而为大多数用户保障网络的资源，提高网络的服务品质。更加合理地管理网络资源就可以提高了用户业务感受度，更加优化网络的资源利用。

- 有利运营商控制实时和增值业务。运营商可以为一些实时的业务提供带宽保障，从而为PS业务和网络的发展提供更大的成长空间。

3GPP R7定义了一套PCC架构为运营商提供差异化管控的支撑，可以有效牵引现网有限的带宽向价值用户和价值业务倾斜，从而提升运营商的盈利效能，达到对分组业务进行合理有效控制的效果。

## 2. PCRF架构
PCC（Policy and Charging Control 策略与计费控制）架构是3GPP组织将标准体系R7版本将**策略控制和基于流的计费进行融合**后提出来的架构体系。用于分组网络业务数据传输QoS等策略控制和流计费的一种技术架构。PCC架构工作在SDF（Service Data Flow 业务数据流）级上，提供策略控制、计费控制功能、和业务数据流的事件报告等功能。旨在为用户提供差异化的服务,提供用户业务流承载资源保障以及流计费策略。让运营商实现基于业务和用户分类的更精细化的业务控制和计费方式，以合理利用网络资源，创造最大利润。PCC架构工作在SDF（Service Data Flow 业务数据流）级上，提供策略控制、计费控制功能、和业务数据流的事件报告功能


