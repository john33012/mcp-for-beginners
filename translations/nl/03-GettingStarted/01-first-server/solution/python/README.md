<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "d0f0d7012325b286e4a717791b23ae7e",
  "translation_date": "2025-07-13T18:00:55+00:00",
  "source_file": "03-GettingStarted/01-first-server/solution/python/README.md",
  "language_code": "nl"
}
-->
# Deze sample uitvoeren

Het wordt aanbevolen om `uv` te installeren, maar het is niet verplicht. Zie [instructies](https://docs.astral.sh/uv/#highlights)

## -0- Maak een virtuele omgeving aan

```bash
python -m venv venv
```

## -1- Activeer de virtuele omgeving

```bash
venv\Scrips\activate
```

## -2- Installeer de afhankelijkheden

```bash
pip install "mcp[cli]"
```

## -3- Voer de sample uit

```bash
mcp run server.py
```

## -4- Test de sample

Met de server draaiend in één terminal, open je een andere terminal en voer je het volgende commando uit:

```bash
mcp dev server.py
```

Dit zou een webserver moeten starten met een visuele interface waarmee je de sample kunt testen.

Zodra de server verbonden is:

- probeer tools te tonen en voer `add` uit met argumenten 2 en 4, je zou 6 als resultaat moeten zien.

- ga naar resources en resource template en roep get_greeting aan, typ een naam in en je zou een begroeting met de opgegeven naam moeten zien.

### Testen in CLI-modus

De inspector die je hebt gestart is eigenlijk een Node.js-app en `mcp dev` is een wrapper daaromheen.

Je kunt het direct in CLI-modus starten door het volgende commando uit te voeren:

```bash
npx @modelcontextprotocol/inspector --cli mcp run server.py --method tools/list
```

Dit toont alle beschikbare tools op de server. Je zou de volgende output moeten zien:

```text
{
  "tools": [
    {
      "name": "add",
      "description": "Add two numbers",
      "inputSchema": {
        "type": "object",
        "properties": {
          "a": {
            "title": "A",
            "type": "integer"
          },
          "b": {
            "title": "B",
            "type": "integer"
          }
        },
        "required": [
          "a",
          "b"
        ],
        "title": "addArguments"
      }
    }
  ]
}
```

Om een tool aan te roepen typ je:

```bash
npx @modelcontextprotocol/inspector --cli mcp run server.py --method tools/call --tool-name add --tool-arg a=1 --tool-arg b=2
```

Je zou de volgende output moeten zien:

```text
{
  "content": [
    {
      "type": "text",
      "text": "3"
    }
  ],
  "isError": false
}
```

> ![!TIP]
> Het is meestal veel sneller om de inspector in CLI-modus te draaien dan in de browser.
> Lees meer over de inspector [hier](https://github.com/modelcontextprotocol/inspector).

**Disclaimer**:  
Dit document is vertaald met behulp van de AI-vertalingsdienst [Co-op Translator](https://github.com/Azure/co-op-translator). Hoewel we streven naar nauwkeurigheid, dient u er rekening mee te houden dat geautomatiseerde vertalingen fouten of onnauwkeurigheden kunnen bevatten. Het originele document in de oorspronkelijke taal moet als de gezaghebbende bron worden beschouwd. Voor cruciale informatie wordt professionele menselijke vertaling aanbevolen. Wij zijn niet aansprakelijk voor eventuele misverstanden of verkeerde interpretaties die voortvloeien uit het gebruik van deze vertaling.