# Hexadata Tools — business validation MCP server for AI agents

[Hexadata Tools](https://apify.com/hexadatalabs/business-validation-tools-mcp) is a remote MCP server (Streamable HTTP, hosted on Apify) with **15 deterministic tools** that an AI agent should call instead of guessing: check digits, official tables and official calculations. No LLM inside: the same input always gives the same answer, nothing is stored, and you pay per call ($0.005 per single call, $0.002 per item in batches; invalid input is free).

![The 15 Hexadata Tools: validate, look up, calculate](https://raw.githubusercontent.com/4robot/hexadata-tools-mcp/main/infographic.png)

## Tools

| Tool | Use this when… | Source |
| --- | --- | --- |
| `validate_tax_id` | you need to check a company/taxpayer ID from MX, ES, AR, CL, CO, BR, GB, US, IT, FR, DE, PE, CA, AU or IN (RFC, NIF/CIF, CUIT, RUT, NIT, CNPJ/CPF, UK VAT, EIN, Partita IVA, SIREN/SIRET, USt-IdNr, RUC, BN, ABN/ACN, GSTIN) | official check-digit algorithms |
| `validate_vat_eu` | you need to confirm an EU VAT number is registered (reverse charge) | VIES (European Commission) |
| `validate_iban` | you need to validate an IBAN and split bank/branch/account | ISO 13616 + IBAN registry |
| `lookup_lei` | you need the legal entity behind an LEI, or the LEI of a company name | GLEIF |
| `lookup_postal_code` | you need city/region/coordinates for a postal code in 120+ countries (Mexico with colonia/municipio/estado) | GeoNames (CC BY) |
| `validate_bank_account` | you need to verify a Mexican CLABE (with bank name), a US ABA routing number or a SWIFT/BIC | Banxico SPEI list, ABA rules, ISO 9362 |
| `normalize_phone` | you need a phone number in E.164 with country and line type | libphonenumber |
| `validate_email_domain` | you need to know if an e-mail is well-formed and its domain receives mail (MX, disposable, free provider) | DNS + open blocklist |
| `get_fx_rate` | you need an official exchange rate for a date (30+ currencies, cross rates) | ECB, Banxico FIX |
| `business_days` | you need to add/count business days or list public holidays in 200+ countries | Nager.Date, Mexican LFT art. 74 |
| `lookup_industry_code` | you need a NAICS, NACE, CPV or SCIAN code for an activity, or its hierarchy | Census, Eurostat, EU Publications Office, INEGI |
| `lookup_hs_code` | you need a customs tariff code (US HTS 10 digits, EU CN8) and the US duty rate | USITC, Eurostat |
| `get_vat_rates` | you need the standard and reduced VAT/GST rates of 35 countries for a date | TEDB + national laws (versioned table) |
| `lookup_sat_catalog` | you need a Mexican CFDI 4.0 catalog value (product key, unit, CFDI use, tax regime, payment form/method…) | SAT official workbook |
| `calc_payroll_mx` | you need Mexican ISR withholding, employment subsidy, IMSS per branch, Infonavit and net pay for a salary | DOF, INEGI, Ley del Seguro Social (2026 tables) |

## Connect in one step

Direct server URL (Streamable HTTP):

```text
https://hexadatalabs--business-validation-tools-mcp.apify.actor/mcp
```

Authentication: `Authorization: Bearer <APIFY_TOKEN>` (free Apify account with monthly credit, or an x402 prepaid token bought with USDC, no account needed).

Or through the Apify MCP server, next to any other Actor:

```text
https://mcp.apify.com?tools=hexadatalabs/business-validation-tools-mcp
```

### Claude Desktop, Cursor, Windsurf, VS Code (JSON config)

```json
{
  "mcpServers": {
    "hexadata-validation": {
      "url": "https://hexadatalabs--business-validation-tools-mcp.apify.actor/mcp",
      "headers": { "Authorization": "Bearer <APIFY_TOKEN>" }
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http hexadata-validation https://hexadatalabs--business-validation-tools-mcp.apify.actor/mcp --header "Authorization: Bearer <APIFY_TOKEN>"
```

## Example prompts

- "Validate these supplier tax IDs: MX EKU9003173C9, ES B12345674 and BR 11.222.333/0001-81, and tell me which are malformed."
- "Is IE6388047V registered for VAT, and what company is it?"
- "Check this IBAN before I pay: DE89 3704 0044 0532 0130 00. Which bank code is it?"
- "Which SAT product key and unit should I use to invoice software development services in Mexico, and what does a 20,000 MXN monthly salary cost the employer in 2026?"
- "What is the HTS code and US duty rate for stainless steel kitchen sinks, and the VAT rate in Germany today?"
- "Add 10 business days to 2026-12-18 in Mexico and list the holidays skipped."

## Pricing

$0.005 per single-item call, $0.002 per item when a call carries 2–500 items. Items answered with `INVALID_INPUT`, `UNSUPPORTED_COUNTRY` or `SOURCE_UNAVAILABLE` are never charged. 1,000 IBAN checks in batches ≈ $2; 100 Mexican payroll calculations ≈ $0.20.

## Links

- Actor page and full documentation: https://apify.com/hexadatalabs/business-validation-tools-mcp
- All Hexadata Actors: https://apify.com/hexadatalabs
- Issues and feature requests (new countries, catalogs, code systems): the Issues tab of the Actor.
