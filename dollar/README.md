
# Dollar Update Blueprint

## Overview

The "Dollar Update" blueprint is designed to fetch the latest US Dollar exchange rates from the Brazilian Central Bank and send an email with the updated information. This blueprint utilizes several modules to achieve the following:

1. **Fetch Data**: Retrieve the latest exchange rate data from the Central Bank of Brazil.
2. **Parse JSON**: Process the fetched JSON data.
3. **Set Variables**: Extract and format specific data fields.
4. **Send Email**: Compose and send an email containing the exchange rate details.

## Flow

### 1. Fetch Data (`http:ActionSendData`)
This module sends an HTTP GET request to the Brazilian Central Bank API to fetch the exchange rate data for the current date.

- **Endpoint**: `https://olinda.bcb.gov.br/olinda/servico/PTAX/versao/v1/odata/CotacaoDolarDia(dataCotacao=@dataCotacao)?@dataCotacao='{{formatDate(now; "MM-DD-YYYY")}}'&$top=100&$format=json&$select=cotacaoCompra,cotacaoVenda,dataHoraCotacao`
- **Method**: GET
- **Response**: JSON containing `cotacaoCompra`, `cotacaoVenda`, and `dataHoraCotacao`.

### 2. Parse JSON (`json:ParseJSON`)
This module parses the JSON response received from the API.

- **Input**: JSON string from the HTTP request.
- **Output**: Extracted fields: `cotacaoCompra`, `cotacaoVenda`, and `dataHoraCotacao`.

### 3. Set Variables (`util:SetVariables`)
This module sets the extracted values into variables for further use.

- **Variables**:
  - `Date`: Formatted date from `dataHoraCotacao`.
  - `Time`: Formatted time from `dataHoraCotacao`.

### 4. Send Email (`microsoft-email:createAndSendAMessage`)
This module sends an email with the formatted exchange rate information.

- **Email Subject**: "Cotação do dolar atualizada"
- **Email Body**:
  ```html
  <HTML>
  <b>Bid:</b> {{7.value[].cotacaoCompra}}<br>
  <b>Ask:</b> {{7.value[].cotacaoVenda}}<br>
  <b>Date:</b> {{12.Date}}<br>
  <b>Time:</b> {{12.Time}}<br>
  </HTML>
  ```

## Usage

### Prerequisites
- Make.com account
- Microsoft account with email sending permissions

### Steps

1. **Import the Blueprint**:
   - Log in to your Make.com account.
   - Import the blueprint by copying the provided JSON and pasting it into the Make.com scenario editor.

2. **Configure HTTP Module**:
   - Ensure the HTTP module is correctly configured with the endpoint and parameters provided in the blueprint.

3. **Set Up Microsoft Email Module**:
   - Configure the Microsoft Email module with your Microsoft account.
   - Set the recipient email address in the `toRecipients` field of the email module.

4. **Run the Scenario**:
   - Execute the scenario to fetch the latest exchange rate data and send an email with the details.

### Notes
- The blueprint is designed to handle errors gracefully and retry up to three times in case of failures.
- Ensure your Make.com account has the necessary permissions and credits to run the HTTP requests and send emails.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Contact
For any questions or support, please contact [Your Name] at [Your Email].
