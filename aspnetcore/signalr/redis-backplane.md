---
title: Redis propojovacího rozhraní pro horizontální navýšení kapacity funkce SignalR technologie ASP.NET Core
author: tdykstra
description: Zjistěte, jak nastavit propojovací rozhraní Redis umožňuje škálování aplikace SignalR technologie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2018
ms.locfileid: "52453011"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>Nastavit propojovací rozhraní Redis pro horizontální navýšení kapacity funkce SignalR technologie ASP.NET Core

Podle [Andrew Stanton sestry](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), a [Petr Dykstra](https://github.com/tdykstra),

Tento článek vysvětluje aspekty SignalR konkrétní nastavení [Redis](https://redis.io/) server pro horizontální navýšení kapacity aplikace SignalR technologie ASP.NET Core.

## <a name="set-up-a-redis-backplane"></a>Nastavit propojovací rozhraní Redis

* Nasazení serveru Redis.

  Pro použití v produkčním prostředí propojovací rozhraní Redis je doporučeno pouze pro místní infrastrukturu. Abyste minimalizovali latenci serveru Redis by měl být ve stejném datovém centru jako aplikace SignalR. Pokud vaše aplikace SignalR běží v cloudu Azure, doporučujeme namísto Redis propojovací rozhraní služby Azure SignalR. Můžete použít službu Azure Redis Cache pro vývojové a testovací prostředí. Další informace naleznete v následujících materiálech:

  * <xref:signalr/scale>
  * [Dokumentace ke službě redis](https://redis.io/)
  * [Dokumentace ke službě Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* V aplikaci SignalR, nainstalujte `Microsoft.AspNetCore.SignalR.Redis` balíček NuGet.

* V `Startup.ConfigureServices` metody, volání `AddRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* Podle potřeby nakonfigurujte možnosti:
 
  Většinu možností lze nastavit v připojovacím řetězci nebo v [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objektu. Možnosti zadané v `ConfigurationOptions` přepsat ty nastavit v připojovacím řetězci.

  Následující příklad ukazuje, jak nastavit možnosti `ConfigurationOptions` objektu. V tomto příkladu přidá předponu kanál tak, aby stejné instance Redis může sdílet více aplikací, jak je popsáno v následujícím kroku.

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  V předchozím kódu `options.Configuration` je inicializována s cokoli, co byl zadán v připojovacím řetězci.

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* V aplikaci SignalR, nainstalujte `Microsoft.AspNetCore.SignalR.StackExchangeRedis` balíček NuGet.

* V `Startup.ConfigureServices` metody, volání `AddStackExchangeRedis` po `AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* Podle potřeby nakonfigurujte možnosti:
 
  Většinu možností lze nastavit v připojovacím řetězci nebo v [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) objektu. Možnosti zadané v `ConfigurationOptions` přepsat ty nastavit v připojovacím řetězci.

  Následující příklad ukazuje, jak nastavit možnosti `ConfigurationOptions` objektu. V tomto příkladu přidá předponu kanál tak, aby stejné instance Redis může sdílet více aplikací, jak je popsáno v následujícím kroku.

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  V předchozím kódu `options.Configuration` je inicializována s cokoli, co byl zadán v připojovacím řetězci.

  Informace o možnostech Redis, najdete v článku [StackExchange Redis dokumentaci](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).

::: moniker-end

* Pokud používáte jeden server Redis pro více aplikací SignalR, použijte předponu jiný kanál pro každou aplikaci SignalR.

  Nastavení kanálu předponu izoluje jedna aplikace SignalR od ostatních, které používají jiný kanál předpony. Pokud nechcete přiřadit odlišné předpony, zpráv odesílaných z jedné aplikace do všech svých vlastních klientů přejdete na všechny klienty ze všech aplikací, které používají Redis server jako propojovací rozhraní.

* Konfigurace vašeho serveru farmy Vyrovnávání zatížení softwaru pro rychlé relace. Tady je několik příkladů dokumentace o tom, jak to udělat:

  * [SLUŽBA IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [Haproxy:](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Server Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Chyby serveru redis

Když Redis server přestane fungovat, SignalR vyvolá výjimky, které označují, že se nebudou doručovat zprávy. Některé typické výjimka zprávy:

* *Neúspěšné zapisované zprávě*
* *Nepovedlo se vyvolat metodu rozbočovače na "MethodName.*
* *Připojení k Redis se nezdařilo*

Funkce SignalR nemá vyrovnávací paměť zprávy k odeslání je při přechodu serveru zpět. Všechny zprávy odeslané při odstávce serveru Redis se ztratí.

SignalR automaticky znovu připojí, když Redis server je opět k dispozici.

### <a name="custom-behavior-for-connection-failures"></a>Vlastní chování pro chyby připojení

Tady je příklad, který ukazuje, jak zpracovávat události selhání připojení Redis.

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="clustering"></a>Vytváření clusterů

Clustering je metoda pro dosažení vysoké dostupnosti s využitím více serverů Redis. Vytváření clusterů není oficiálně podporován, ale může fungovat.

## <a name="next-steps"></a>Další kroky

Další informace naleznete v následujících materiálech:

* <xref:signalr/scale>
* [Dokumentace ke službě redis](https://redis.io/documentation)
* [Dokumentace ke službě StackExchange Redis](https://stackexchange.github.io/StackExchange.Redis/)
* [Dokumentace ke službě Azure Redis Cache](https://docs.microsoft.com/en-us/azure/redis-cache/)