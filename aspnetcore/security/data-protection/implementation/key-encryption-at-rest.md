---
title: Szyfrowanie kluczy przechowywanych w ASP.NET Core
author: rick-anderson
description: Szczegóły dotyczące wykonania szyfrowania kluczy platformy ASP.NET Core do ochrony danych magazynowanych.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: e5082d831dd4822fad0fb3211fe2b8c76ff967bf
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Szyfrowanie kluczy przechowywanych w ASP.NET Core

<a name="data-protection-implementation-key-encryption-at-rest"></a>

Domyślnie system ochrony danych [wykorzystuje heurystyki](xref:security/data-protection/configuration/default-settings) ustalenie, jak kryptograficznych materiału klucza powinny być szyfrowane, gdy. Deweloper może zastąpić heurystyki i ręcznie określić, jak klucze powinny być szyfrowane, gdy.

> [!NOTE]
> Jeśli określisz jawne klucza szyfrowania w mechanizmu reszta systemu ochrony danych będą wyrejestrowania domyślnego mechanizmu magazynu kluczy heurystyki podane. Należy [Określ mechanizmu magazynowania kluczy jawne](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), w przeciwnym razie system ochrony danych nie powiedzie się.

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

System ochrony danych jest dostarczany z trzech mechanizmów szyfrowania w polu.

## <a name="windows-dpapi"></a>DPAPI systemu Windows

*Ten mechanizm jest dostępna tylko w systemie Windows.*

W przypadku systemu Windows DPAPI materiału klucza będą szyfrowane za pomocą [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) przed jest utrwalana w magazynie. DPAPI to mechanizm szyfrowania odpowiednie dane, które nigdy nie będzie można odczytać poza bieżącym komputerze (mimo że można utworzyć kopię tych kluczy do usługi Active Directory, zobacz [DPAPI i profili mobilnych](https://support.microsoft.com/kb/309408/#6)). Na przykład skonfigurować szyfrowanie klucza na rest DPAPI.

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

Jeśli `ProtectKeysWithDpapi` jest wywoływana bez parametrów, tylko konta użytkownika systemu Windows można odszyfrować utrwalonego materiału klucza. Opcjonalnie możesz określić dowolne konto użytkownika na komputerze (nie tylko bieżące konto użytkownika) należy stanie odszyfrować materiał kluczy, jak pokazano w poniższym przykładzie.

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a>Certyfikat X.509

*Ten mechanizm nie jest dostępna w `.NET Core 1.0` lub `1.1`.*

Jeśli aplikacja zostanie rozmieszczona na wielu komputerach, może być wygodne rozpowszechniają udostępnionego certyfikatu X.509 maszyn i konfiguracji aplikacji, aby użyć tego certyfikatu do szyfrowania kluczy w stanie spoczynku. Poniżej znajduje się przykład.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

Obsługiwane są tylko certyfikatów z kluczami prywatnymi CAPI ze względu na ograniczenia .NET Framework. Zobacz [szyfrowania opartego na certyfikatach z DPAPI NG Windows](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) poniżej dla możliwe obejścia tych ograniczeń.

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

*Ten mechanizm jest dostępna tylko w systemie Windows 8 / Windows Server 2012 lub nowszym.*

Począwszy od systemu Windows 8, system operacyjny obsługuje DPAPI-NG (nazywanych również CNG DPAPI). Microsoft wychodzi poza jego scenariusz użycia w następujący sposób.

   Rozwiązań w chmurze, często wymaga tej zawartości zaszyfrowane na jednym komputerze można odszyfrować na innym. W związku z tym począwszy od systemu Windows 8, Microsoft rozszerzony pomysł obejmują scenariusze chmury przy użyciu stosunkowo prosta interfejsu API. Ten nowy interfejs API, nazywany DPAPI NG, umożliwia bezpieczne udostępnianie kluczy tajnych (klucze, hasła, materiału klucza) i komunikaty, aby chronić je do zestawu podmiotów zabezpieczeń, które mogą służyć do usunięcia ochrony je na różnych komputerach po odpowiedniego uwierzytelnienia i autoryzacji.

   Z [o CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)

Podmiot zabezpieczeń został zakodowany jako regułę ochrony deskryptora. Należy wziąć pod uwagę poniższym przykładzie, który szyfruje materiału klucza tak, aby tylko przyłączonych do domeny użytkownika z określonym identyfikatorem SID może odszyfrować materiału klucza.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

Istnieje też bezparametrowy przeciążenia `ProtectKeysWithDpapiNG`. Jest to metoda wygody służącą do reguły "identyfikatora SID = min", gdzie analizę jest identyfikator SID konta użytkownika systemu Windows.

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

W tym scenariuszu kontroler domeny usługi AD jest odpowiedzialna za dystrybucję kluczy szyfrowania używanych przez operacje DPAPI NG. Użytkownik docelowy będzie można odszyfrować zaszyfrowanego ładunku z dowolnego komputera przyłączonych do domeny (pod warunkiem, że proces jest uruchomiony w ramach ich tożsamości).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Na podstawie certyfikatu szyfrowania z DPAPI NG systemu Windows

Jeśli pracujesz na Windows 8.1 / Windows Server 2012 R2 lub później, umożliwia Windows DPAPI-NG szyfrowanie oparte na certyfikatach, nawet wtedy, gdy aplikacja jest uruchomiona na .NET Core. Aby skorzystać z tego, użyj ciągu deskryptora reguły "certyfikatu = HashId:thumbprint", gdzie odcisk palca jest kodowany w formacie hex SHA1 odcisk palca certyfikatu do użycia. Poniżej znajduje się przykład.

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

Dowolnej aplikacji, która jest wskazywana w tym repozytorium musi być uruchomiona na Windows 8.1 / Windows Server 2012 R2 lub nowszej, aby być w stanie odszyfrować tego klucza.

## <a name="custom-key-encryption"></a>Niestandardowe klucza szyfrowania

Jeśli mechanizmów w polu nie są odpowiednie, deweloper można określić własnych mechanizmów szyfrowania zapewniając niestandardowego `IXmlEncryptor`.
