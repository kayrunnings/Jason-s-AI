# Miro Integration Guide

This guide explains how to use Miro visuals as input for your AI workflow phases.

## Overview

Miro boards can provide valuable visual context for any workflow phase:

| Phase | Miro Input Examples |
|-------|---------------------|
| **Research** | Mind maps, competitive analysis boards, user journey maps |
| **PRD (Product)** | Feature flowcharts, architecture diagrams, user stories |
| **CRD (Content)** | Content hierarchy maps, messaging frameworks, tone boards |
| **DRD (Design)** | Wireframes, mockups, component layouts, style tiles |

---

## Configuration

### Environment Variables

Your Miro credentials are stored in `.env`:

```bash
MIRO_ACCESS_TOKEN=your_token_here
MIRO_BOARD_ID=your_default_board_id
MIRO_API_BASE_URL=https://api.miro.com/v2
```

### Finding Board & Frame IDs

**Board ID** - From the URL:
```
https://miro.com/app/board/uXjVGZRKxQM=/
                          └─────────────┘
                            Board ID
```

**Frame ID** - Right-click a frame → "Copy link" → extract ID from URL:
```
https://miro.com/app/board/uXjVGZRKxQM=/?moveToWidget=1234567890
                                                      └────────┘
                                                       Frame ID
```

---

## How to Use Miro Visuals as Input

### Method 1: Provide Board/Frame Reference

When starting any phase, include the Miro reference:

```
Phase: Create DRD
Miro Input: https://miro.com/app/board/uXjVGZRKxQM=/
Frame: "Homepage Wireframe"

Please analyze the wireframe and create a Design Requirements Document.
```

### Method 2: Export and Attach Image

1. In Miro, select the frame/content
2. Right-click → "Export" → "Export as image"
3. Provide the exported image as input to the phase

### Method 3: API-Based Content Extraction

Use the Miro API to programmatically extract board content:

```bash
# Get all items from a board
curl -H "Authorization: Bearer $MIRO_ACCESS_TOKEN" \
  "$MIRO_API_BASE_URL/boards/$MIRO_BOARD_ID/items"

# Get a specific frame's content
curl -H "Authorization: Bearer $MIRO_ACCESS_TOKEN" \
  "$MIRO_API_BASE_URL/boards/$MIRO_BOARD_ID/items?parent_item_id=FRAME_ID"
```

---

## Phase-Specific Usage

### Research Phase (research.md)

**Useful Miro inputs:**
- Competitive analysis boards
- Mind maps of problem space
- User interview synthesis
- Stakeholder maps

**Example prompt:**
```
@research.md

Miro Board: https://miro.com/app/board/uXjVGZRKxQM=/
Frame: "Competitive Analysis"

Research how competitors handle user authentication flows.
Use the competitive analysis board as a starting point.
```

### PRD Phase (create-prd.md)

**Useful Miro inputs:**
- Feature flowcharts
- User journey maps
- System architecture diagrams
- Story mapping boards

**Example prompt:**
```
@create-prd.md

Miro Board: https://miro.com/app/board/uXjVGZRKxQM=/
Frame: "Checkout Flow Diagram"

Create a PRD for the checkout feature based on the flow diagram.
```

### CRD Phase (create-crd.md)

**Useful Miro inputs:**
- Content hierarchy maps
- Brand messaging frameworks
- Information architecture
- Tone/voice mood boards

**Example prompt:**
```
@create-crd.md

Miro Board: https://miro.com/app/board/uXjVGZRKxQM=/
Frame: "Email Campaign Structure"

Create a CRD for the onboarding email series based on the content map.
```

### DRD Phase (create-drd.md)

**Useful Miro inputs:**
- Wireframes and mockups
- Component layouts
- Style tiles
- Interaction flows
- Responsive breakpoint designs

**Example prompt:**
```
@create-drd.md

Miro Board: https://miro.com/app/board/uXjVGZRKxQM=/
Frame: "Dashboard Wireframe v2"

Create a DRD for the analytics dashboard based on the wireframe.
Include all components visible in the frame.
```

---

## API Reference

### Common Endpoints

| Endpoint | Purpose |
|----------|---------|
| `GET /boards/{board_id}` | Get board details |
| `GET /boards/{board_id}/items` | List all items on board |
| `GET /boards/{board_id}/frames` | List all frames |
| `GET /boards/{board_id}/images` | List all images |
| `GET /boards/{board_id}/sticky_notes` | List sticky notes |
| `GET /boards/{board_id}/shapes` | List shapes |

### Export Board as Image

```bash
# Export entire board
curl -H "Authorization: Bearer $MIRO_ACCESS_TOKEN" \
  "$MIRO_API_BASE_URL/boards/$MIRO_BOARD_ID/export" \
  -d '{"format": "png"}' \
  -o board_export.png
```

---

## Best Practices

1. **Use Frames** - Organize your Miro board with frames for different features/sections
2. **Name Frames Clearly** - Use descriptive names like "Homepage Wireframe v2"
3. **Keep Visuals Clean** - Remove clutter before using as input
4. **Version Your Designs** - Use frame names or tags to track versions
5. **Link to Specific Frames** - Reference exact frames rather than entire boards

---

## Troubleshooting

### "Unauthorized" Error
- Check your access token hasn't expired
- Verify the token has `boards:read` scope
- Ensure you have access to the board

### "Board Not Found" Error
- Verify the board ID is correct
- Check the board hasn't been deleted
- Ensure your token's team has board access

### Rate Limiting
- Miro API has rate limits (varies by plan)
- Add delays between bulk requests
- Cache responses when possible
