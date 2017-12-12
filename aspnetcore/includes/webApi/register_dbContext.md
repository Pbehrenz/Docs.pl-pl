## <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

W tym kroku kontekst bazy danych jest zarejestrowany w [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera. Usługi (takie jak kontekst bazy danych), które są zarejestrowane w usłudze kontenera iniekcji (Podpisane) zależności są dostępne do kontrolerów.

Zarejestruj kontekst bazy danych z kontenerem usługi przy użyciu wbudowaną obsługę [iniekcji zależności](xref:fundamentals/dependency-injection). Zastąp zawartość *Startup.cs* pliku następującym kodem:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Poprzedni kod:

* Usunięcie kodu, który nie jest używany.
* Określa, że bazy danych w pamięci są wstrzykiwane do kontenera usług.