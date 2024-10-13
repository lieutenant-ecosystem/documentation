# How to Deploy

## Infrastructure

Use the `Docker Compose` file below:
```yaml
version: '3.8'
services:

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    ports:
      - "8080:8080"
    volumes:
      - open-webui:/app/backend/data
    restart: always
    environment:
      - ENV=${ENV}
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - GOOGLE_PSE_API_KEY=${GOOGLE_PSE_API_KEY}
      - GOOGLE_PSE_ENGINE_ID=${GOOGLE_PSE_ENGINE_ID}
      - MICROSOFT_CLIENT_ID=${MICROSOFT_CLIENT_ID}
      - MICROSOFT_CLIENT_SECRET=${MICROSOFT_CLIENT_SECRET}
      - MICROSOFT_CLIENT_TENANT_ID=${MICROSOFT_CLIENT_TENANT_ID}
      - DATABASE_URL=postgresql://openwebui:unsecuredpassword@db:5432/openwebui
    depends_on:
      - db

  db:
    image: postgres:latest
    container_name: relational_database
    environment:
      POSTGRES_DB: openwebui
      POSTGRES_USER: openwebui
      POSTGRES_PASSWORD: unsecuredpassword
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  open-webui:
  postgres_data:

```

## Configurations

| Key                          | Description                                                                                                                                                                              |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ENV`                        | Either `prod` or `dev`                                                                                                                                                                   |
| `OPENAI_API_KEY`             | OpenAI API Key                                                                                                                                                                           |
| `GOOGLE_PSE_API_KEY`         | Google Programmable Search Engine API Key ([guide](https://developers.google.com/custom-search/v1/introduction#identify_your_application_to_google_with_api_key))                        |
| `GOOGLE_PSE_ENGINE_ID`       | Google Programmable Search Engine ID                                                                                                                                                     |
| `MICROSOFT_CLIENT_ID`        | Microsoft Entra ID Client ID ([guide](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory#create-the-app-registration)) |
| `MICROSOFT_CLIENT_SECRET`    | Microsoft Entra ID Client Secret                                                                                                                                                         |
| `MICROSOFT_CLIENT_TENANT_ID` | Microsoft Entra ID Tenant ID                                                                                                                                                             |