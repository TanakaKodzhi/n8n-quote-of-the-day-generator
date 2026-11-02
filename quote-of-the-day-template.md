{
  "name": "Генератор картинок «Цитата дня» для соцсетей",
  "nodes": [
    {
      "parameters": {
        "content": "**Проект: Генератор картинок «Цитата дня» для соцсетей**\n\n**Суть:** Раз в день бот берет случайную умную цитату из бесплатного API, отправляет этот текст в нейросеть DeepSeek с запросом на перевод, разбор и генерацию промпта, генерирует фон через Pollinations AI и публикует оформленный пост с картинкой в Telegram-канал.",
        "height": 240,
        "width": 360,
        "color": 4
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -140,
        -420
      ],
      "id": "bb850ad3-ce78-4073-9f83-445a526153a6",
      "name": "Sticky Note - Описание"
    },
    {
      "parameters": {
        "content": "### 🚀 Инструкция по настройке:\n\n1. **DeepSeek API:**\n   - Откройте ноды `DeepSeek Chat Model` и `DeepSeek Chat Model1`.\n   - Привяжите свой аккаунт `DeepSeek API` (API-ключ с платформы platform.deepseek.com).\n\n2. **Telegram Bot:**\n   - В ноде `Send a photo message` укажите свои учетные данные `Telegram API` (токен от @BotFather).\n   - Замените параметр `Chat ID` на ID вашего канала (например, `-100xxxxxxxxxx`) или `@имя_канала`.\n   - Добавьте бота в администраторы канала с правом публикации сообщений.\n\n3. **Расписание:**\n   - В ноде `Schedule Trigger` проверьте часовой пояс и время запуска (по умолчанию 07:00).",
        "height": 340,
        "width": 460,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        260,
        -420
      ],
      "id": "f29b4e11-8c52-4a9f-9d33-bc2749d97a10",
      "name": "Sticky Note - Инструкция"
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "triggerAtHour": 7
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.4,
      "position": [
        -48,
        -144
      ],
      "id": "d93745ba-4615-44c1-a9c4-d1086d1135fd",
      "name": "Schedule Trigger"
    },
    {
      "parameters": {
        "url": "https://dummyjson.com/quotes/random",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        176,
        -144
      ],
      "id": "12d4f776-74ee-4b4c-a058-4c53e67f0bd9",
      "name": "HTTP Request"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Ты — профессиональный редактор контента и специалист по генерации визуальных промптов.\n\nТвоя задача — проанализировать переданную цитату и вернуть результат СТРОГО в формате валидного JSON-объекта со следующей структурой полей:\n\n{\n\"russian_quote\": \"Точный перевод цитаты на русский язык\",\n\"author\": \"Имя автора цитаты\",\n\"meaning\": \"Краткий разбор сути и практического смысла мысли исключительно на русском языке (2-3 предложения)\",\n\"visual_prompt\": \"Детальный промпт для генерации эстетичного фона исключительно на английском языке\"\n}\n\nПравила формирования полей:\n\n\"russian_quote\": художественный, но точный перевод цитаты на чистый русский язык.\n\n\"author\": имя автора цитаты. Если передано на английском — переведи на русский или оставь оригинальное написание (в зависимости от твоей задачи).\n\n\"meaning\": пиши ТОЛЬКО на русском языке. Запрещено вставлять в это поле английский текст или описания visual prompt.\n\n\"visual_prompt\": пиши ТОЛЬКО на английском языке. Опиши кинематографичную, эстетичную сцену, передающую настроение цитаты. Запрещено включать текст, цитаты, надписи, логотипы, кавычки и любые буквы на самом изображении.\n\nЗапрещено смешивать языки внутри полей: \"meaning\" — строго русский, \"visual_prompt\" — строго английский.\n\nВходные данные:\nЦитата: {{ $json.quote }}\nАвтор: {{ $json.author }}\n\nФормат ответа:\nВерни ТОЛЬКО сырой валидный JSON. Никаких префиксов, никаких постфиксов, никаких блоков с тройными кавычками ```json ```.",
        "hasOutputParser": true,
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        384,
        -160
      ],
      "id": "ff056d39-d9e8-4e2a-82ac-f4272a2d6f35",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "jsonSchemaExample": "{\n  \"russian_quote\": \"Единственный способ делать великие дела — любить то, что вы делаете.\",\n  \"author\": \"Rumi\",\n  \"meaning\": \"Истинный успех и выдающиеся результаты невозможны из-под палки. Только искренняя страсть к своему делу даёт энергию преодолевать трудности, рутину и продолжать движение вперёд, когда другие опускают руки.\",\n  \"visual_prompt\": \"Cinematic and detailed prompt in English describing an aesthetic background scene for image generation, no text, no quotes\"\n}",
        "autoFix": true
      },
      "type": "@n8n/n8n-nodes-langchain.outputParserStructured",
      "typeVersion": 1.3,
      "position": [
        560,
        16
      ],
      "id": "a361c415-d4e3-4d47-b4c1-cac50e297f2b",
      "name": "Structured Output Parser"
    },
    {
      "parameters": {
        "model": "deepseek-chat",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatDeepSeek",
      "typeVersion": 1,
      "position": [
        240,
        48
      ],
      "id": "e1a2c18b-0598-4200-9222-3b1cbccf2ba4",
      "name": "DeepSeek Chat Model"
    },
    {
      "parameters": {
        "model": "deepseek-chat",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatDeepSeek",
      "typeVersion": 1,
      "position": [
        432,
        80
      ],
      "id": "30b05623-860b-4c3f-b76e-bbdc22936a49",
      "name": "DeepSeek Chat Model1"
    },
    {
      "parameters": {
        "url": "=https://image.pollinations.ai/prompt/{{ encodeURIComponent($json.output.visual_prompt) }}",
        "options": {
          "response": {
            "response": {
              "responseFormat": "file"
            }
          }
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        752,
        -160
      ],
      "id": "744a8b3b-0e33-4995-9ab4-970f1f431e15",
      "name": "HTTP Request1"
    },
    {
      "parameters": {
        "operation": "sendPhoto",
        "chatId": "YOUR_TELEGRAM_CHAT_ID",
        "binaryData": true,
        "additionalFields": {
          "caption": "=<blockquote>\"{{ $('AI Agent').item.json.output.russian_quote }}\"</blockquote>\n- <b>{{ $('AI Agent').item.json.output.author }}</b>\n\n{{ $('AI Agent').item.json.output.meaning }}",
          "parse_mode": "HTML"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        976,
        -160
      ],
      "id": "c70766a2-d426-420c-93b9-e142962ff497",
      "name": "Send a photo message"
    }
  ],
  "pinData": {},
  "connections": {
    "Schedule Trigger": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Structured Output Parser": {
      "ai_outputParser": [
        [
          {
            "node": "AI Agent",
            "type": "ai_outputParser",
            "index": 0
          }
        ]
      ]
    },
    "DeepSeek Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "DeepSeek Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "Structured Output Parser",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "HTTP Request1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request1": {
      "main": [
        [
          {
            "node": "Send a photo message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send a photo message": {
      "main": [
        []
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "timeSavedMode": "fixed"
  },
  "tags": []
}