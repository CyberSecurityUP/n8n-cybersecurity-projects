{
  "name": "Recon - crt.sh",
  "nodes": [
    {
      "parameters": {
        "jsCode": "// Emite 1 item com o campo 'domain' definido\nreturn [{ json: { domain: \"Example.com\" } }];\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1472,
        160
      ],
      "id": "d7bac004-25d5-4943-84d4-8a06b99f63fa",
      "name": "Set Domain (Code)"
    },
    {
      "parameters": {},
      "id": "35eaeb87-1bb9-44d8-918f-8fe24f5ca015",
      "name": "Next Batch",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -736,
        320
      ]
    },
    {
      "parameters": {},
      "id": "fae97160-fa07-40cf-accc-85eb2f78f6e3",
      "name": "Done",
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        1280,
        64
      ]
    },
    {
      "parameters": {
        "fileName": "recon.csv",
        "options": {}
      },
      "id": "e5326f86-a72c-4b9e-982d-e7144e190365",
      "name": "Write File",
      "type": "n8n-nodes-base.writeBinaryFile",
      "typeVersion": 1,
      "position": [
        1104,
        64
      ]
    },
    {
      "parameters": {
        "operation": "toFile",
        "options": {
          "fileName": "recon_{{$now}}.csv"
        }
      },
      "id": "53c14604-ee42-4a6a-88e3-8d3b3308761b",
      "name": "To CSV (Binary)",
      "type": "n8n-nodes-base.spreadsheetFile",
      "typeVersion": 1,
      "position": [
        928,
        64
      ]
    },
    {
      "parameters": {
        "conditions": {
          "boolean": [
            {
              "value1": "={{$json.alive}}",
              "operation": "isTrue"
            }
          ]
        },
        "options": {}
      },
      "id": "ed697f46-625e-4557-8c36-d5aba7d1daee",
      "name": "IF Alive",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2,
      "position": [
        752,
        112
      ]
    },
    {
      "parameters": {
        "batchSize": 25,
        "options": {}
      },
      "id": "49e16313-cce4-4778-a819-9f0ee1a677a6",
      "name": "Split In Batches",
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 1,
      "position": [
        -768,
        160
      ]
    },
    {
      "parameters": {
        "jsCode": "const items = $input.all();\nconst hosts = items.map((item) => item.json.common_name);\nreturn { hosts };\n"
      },
      "id": "86c99cb7-12e7-4a35-bd26-de12e7c6ba3c",
      "name": "Normalize & Dedupe (Code)",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1008,
        160
      ]
    },
    {
      "parameters": {
        "url": "=https://crt.sh/?q=%25.{{$json.domain}}&output=json",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Accept",
              "value": "application/json"
            },
            {
              "name": "User-Agent",
              "value": "n8n-recon/1.0"
            }
          ]
        },
        "options": {
          "timeout": 120000
        }
      },
      "id": "80609780-4884-44d3-989a-c9e841c46be5",
      "name": "Get crt.sh",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4,
      "position": [
        -1248,
        160
      ],
      "executeOnce": false,
      "alwaysOutputData": false
    },
    {
      "parameters": {},
      "id": "b3883b15-2b88-45d7-87d1-85389517f974",
      "name": "Manual Trigger",
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        -1696,
        160
      ]
    }
  ],
  "pinData": {},
  "connections": {
    "Write File": {
      "main": [
        [
          {
            "node": "Done",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "To CSV (Binary)": {
      "main": [
        [
          {
            "node": "Write File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "IF Alive": {
      "main": [
        [
          {
            "node": "To CSV (Binary)",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Next Batch",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Set Domain (Code)": {
      "main": [
        [
          {
            "node": "Get crt.sh",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Next Batch": {
      "main": [
        [
          {
            "node": "Split In Batches",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Split In Batches": {
      "main": [
        [
          {
            "node": "IF Alive",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Normalize & Dedupe (Code)": {
      "main": [
        [
          {
            "node": "Split In Batches",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get crt.sh": {
      "main": [
        [
          {
            "node": "Normalize & Dedupe (Code)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Manual Trigger": {
      "main": [
        [
          {
            "node": "Set Domain (Code)",
            "type": "main",
            "index": 0
          },
          {
            "node": "Get crt.sh",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "ac37fc31-c375-4541-8c97-1e9d60a7a600",
  "meta": {
    "instanceId": "478239603b48d564ccad3b1ff274587a77885f63755077ec6f8360d395f45bed"
  },
  "id": "HrddRDEIocfsU71r",
  "tags": []
}
