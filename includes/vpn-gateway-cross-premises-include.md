|  | **点到站点** | **站点到站点** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Azure 支持的服务** |云服务和虚拟机 |云服务和虚拟机 |[服务列表](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **典型带宽** |通常 < 100 Mbps（总计） |通常 < 100 Mbps（总计） |50 Mbps、100 Mbps、200 Mbps、500 Mbps、1 Gbps、2 Gbps、5 Gbps、10 Gbps |
| **支持的协议** |安全套接字隧道协议 (SSTP) |IPsec |通过 VLAN、NSP 的 VPN 技术（MPLS、VPLS...）直接连接 |
| **路由** |基于路由（动态） |支持基于策略（静态路由）和基于路由（动态路由 VPN） |BGP |
| **连接复原能力** |主动-被动 |主动-被动 |主动-主动 |
| **典型用例** |云服务和虚拟机的原型设计、开发/测试/实验方案 |云服务和虚拟机的开发/测试/实验方案和小规模生产工作负荷 |访问所有 Azure 服务（已验证列表）、企业级和任务关键型工作负荷、备份、大数据、Azure 即 DR 站点 |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **定价** |[定价](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[定价](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[定价](https://azure.microsoft.com/pricing/details/expressroute/) |
| **技术文档** |[VPN 网关文档](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN 网关文档](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute 文档](https://azure.microsoft.com/documentation/services/expressroute/) |
| **常见问题** |[VPN 网关常见问题](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN 网关常见问题](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute 常见问题](../articles/expressroute/expressroute-faqs.md) |



<!--HONumber=Nov16_HO3-->


