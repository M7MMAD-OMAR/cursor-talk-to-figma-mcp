# Cursor Talk to Figma MCP

This project implements a Model Context Protocol (MCP) integration between Cursor AI and Figma, allowing Cursor to communicate with Figma for reading designs and modifying them programmatically.

https://github.com/user-attachments/assets/129a14d2-ed73-470f-9a4c-2240b2a4885c

## Recent Improvements for Windows Users

We've added several improvements to make the project easier to use on Windows:

### PowerShell Execution Policy Fix

The project now includes solutions for the common PowerShell execution policy issues:

- Added `start_server.cmd` - A batch file that runs the WebSocket server with `ExecutionPolicy Bypass`
- Improved setup instructions to include the necessary execution policy commands
- All PowerShell scripts are now designed to work with stricter execution policies

### Automatic WebSocket Server Startup

New automatic startup options have been added:

1. **One-click Startup**: Simply run `start_server.cmd` to start the WebSocket server
2. **VSCode Integration**: Added `.vscode/tasks.json` that automatically starts the server when opening the project
3. **Cursor Integration**: Added `.cursor/settings.json` with startup commands to launch the server in the background

These improvements eliminate the need to manually run PowerShell commands each time you want to use the project.

## Windows Support

Windows support was added by [@Clark934](https://github.com/Clark934) with contributions including:

- PowerShell setup scripts for automated installation
- Windows-specific MCP configuration using environment variables
- Proper path handling for Windows environments
- Documentation for Windows setup and configuration
- Fixed WebSocket server startup for Windows environments

The Windows fork of this project can be found at: https://github.com/Clark934/cursor-talk-to-figma-mcp-windows

## Project Structure

- `src/talk_to_figma_mcp/` - TypeScript MCP server for Figma integration
- `src/cursor_mcp_plugin/` - Figma plugin for communicating with Cursor
- `src/socket.ts` - WebSocket server that facilitates communication between the MCP server and Figma plugin

## Get Started

### Windows Setup

1. Install Bun:

```powershell
powershell -c "irm bun.sh/install.ps1|iex"
```

2. Add Bun to your PowerShell profile (run PowerShell as Administrator):

```powershell
if (!(Test-Path -Path $PROFILE)) {
    New-Item -ItemType File -Path $PROFILE -Force
}
Add-Content -Path $PROFILE -Value "Set-Alias -Name bun -Value `"$env:USERPROFILE\.bun\bin\bun.exe`" -Scope Global"
```

3. Close and reopen PowerShell

4. Clone and setup the project:
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

```powershell
.\scripts\setup.ps1
```

```powershell
git clone https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp.git
cd cursor-talk-to-figma-mcp

```

5. Configure the MCP server in Cursor (create/edit `%USERPROFILE%\.cursor\mcp.json`):

```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "%USERPROFILE%\\.bun\\bin\\bun.exe",
      "args": [
        "C:/path/to/your/cursor-talk-to-figma-mcp/src/talk_to_figma_mcp/server.ts"
      ]
    }
  }
}
```

Replace `C:/path/to/your` with your actual project path.

6. Start the WebSocket server:

```powershell
.\scripts\start.ps1
```

Or use the new one-click solution:

```
start_server.cmd
```

7. Install the Figma Plugin:

   - Open Figma
   - Go to Plugins > Development > Import plugin from manifest
   - Select `src/cursor_mcp_plugin/manifest.json` from your project directory
   - The plugin will appear as "Cursor MCP Plugin" in your development plugins

8. Use the Plugin:
   - Open a Figma file
   - Run the plugin from Plugins > Development > Cursor MCP Plugin
   - Note the channel ID shown in the plugin window
   - In Cursor, use `join_channel` with the shown channel ID to connect

### Linux/Mac Setup

1. Install Bun if you haven't already:

For Linux/Mac:

```bash
curl -fsSL https://bun.sh/install | bash
```

2. Run setup, this will also install MCP in your Cursor's active project

```bash
bun setup
```

3. Start the Websocket server

For Linux/Mac:

```bash
bun start
```

4. Install [Figma Plugin](#figma-plugin)

# Quick Video Tutorial

[![image](images/tutorial.jpg)](https://www.linkedin.com/posts/sonnylazuardi_just-wanted-to-share-my-latest-experiment-activity-7307821553654657024-yrh8)

## Manual Setup and Installation

### MCP Server: Integration with Cursor

The MCP server configuration should be added to your global Cursor configuration:

For Windows (in `%USERPROFILE%\.cursor\mcp.json`):

```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "%USERPROFILE%\\.bun\\bin\\bun.exe",
      "args": [
        "C:/path/to/your/cursor-talk-to-figma-mcp/src/talk_to_figma_mcp/server.ts"
      ]
    }
  }
}
```

For Linux/Mac (in `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "bun",
      "args": [
        "/path/to/cursor-talk-to-figma-mcp/src/talk_to_figma_mcp/server.ts"
      ]
    }
  }
}
```

Note: Replace the paths with your actual project path. Use forward slashes (/) even on Windows.

### WebSocket Server

The WebSocket server must be running for the MCP to communicate with Figma.

For Windows:

```powershell
.\scripts\start.ps1
```

Or use the new one-click solution:

```
start_server.cmd
```

For Linux/Mac:

```bash
bun start
```



## التحسينات الأخيرة لمستخدمي Windows

قمنا بإضافة العديد من التحسينات لجعل المشروع أسهل في الاستخدام على نظام Windows:

### إصلاح سياسة تنفيذ PowerShell

يتضمن المشروع الآن حلولاً لمشاكل سياسة تنفيذ PowerShell الشائعة:

- إضافة ملف `start_server.cmd` - ملف باتش يقوم بتشغيل خادم WebSocket مع `ExecutionPolicy Bypass`
- تحسين تعليمات الإعداد لتشمل أوامر سياسة التنفيذ الضرورية
- تم تصميم جميع نصوص PowerShell للعمل مع سياسات التنفيذ الأكثر صرامة

### التشغيل التلقائي لخادم WebSocket

تمت إضافة خيارات جديدة للتشغيل التلقائي:

1. **التشغيل بنقرة واحدة**: قم ببساطة بتشغيل `start_server.cmd` لبدء خادم WebSocket
2. **تكامل مع VSCode**: تمت إضافة ملف `.vscode/tasks.json` يقوم بتشغيل الخادم تلقائياً عند فتح المشروع
3. **تكامل مع Cursor**: تمت إضافة ملف `.cursor/settings.json` مع أوامر بدء التشغيل لتشغيل الخادم في الخلفية

هذه التحسينات تلغي الحاجة إلى تشغيل أوامر PowerShell يدوياً في كل مرة ترغب في استخدام المشروع.

### التشغيل التلقائي للخادم (Windows)

يتضمن المشروع الآن عدة ملفات لتشغيل خادم WebSocket تلقائياً:

#### `start_server.cmd`

ملف باتش بسيط يقوم بتشغيل خادم WebSocket مع سياسة تنفيذ مناسبة:

```cmd
@echo off
echo Running WebSocket server...
powershell -ExecutionPolicy Bypass -File "%~dp0scripts\start.ps1"
```

طريقة الاستخدام: انقر نقراً مزدوجاً على الملف في مستكشف Windows أو قم بتشغيله من موجه الأوامر.

#### `.vscode/tasks.json`

يقوم هذا الملف بتكوين VSCode/Cursor لبدء تشغيل خادم WebSocket تلقائياً عند فتح مجلد المشروع:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Run WebSocket server",
            "type": "shell",
            "command": "powershell -ExecutionPolicy Bypass -File \"${workspaceFolder}/scripts/start.ps1\"",
            "isBackground": true,
            "problemMatcher": [],
            "presentation": {
                "reveal": "always",
                "panel": "new"
            },
            "runOptions": {
                "runOn": "folderOpen"
            }
        }
    ]
}
```

طريقة الاستخدام: لا يلزم اتخاذ أي إجراء - يبدأ الخادم تلقائياً عند فتح المشروع في VSCode/Cursor.

#### `.cursor/settings.json`

إعدادات خاصة بـ Cursor لتشغيل خادم WebSocket عند فتح المشروع:

```json
{
    "startup": {
        "commands": [
            "powershell -ExecutionPolicy Bypass -WindowStyle Hidden -Command \"Start-Process powershell -ArgumentList '-ExecutionPolicy', 'Bypass', '-File', '\"${workspaceRoot}/scripts/start.ps1\"' -WindowStyle Hidden\""
        ]
    }
}
```

طريقة الاستخدام: لا يلزم اتخاذ أي إجراء - يبدأ الخادم تلقائياً عند فتح المشروع في Cursor.



### Automated Server Startup (Windows)

The project now includes several files to automate the WebSocket server startup:

#### `start_server.cmd`

A simple batch file that starts the WebSocket server with proper execution policy:

```cmd
@echo off
echo Running WebSocket server...
powershell -ExecutionPolicy Bypass -File "%~dp0scripts\start.ps1"
```

Usage: Double-click the file in Windows Explorer or run it from a Command Prompt.

#### `.vscode/tasks.json`

This file configures VSCode/Cursor to automatically start the WebSocket server when opening the project folder:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Run WebSocket server",
            "type": "shell",
            "command": "powershell -ExecutionPolicy Bypass -File \"${workspaceFolder}/scripts/start.ps1\"",
            "isBackground": true,
            "problemMatcher": [],
            "presentation": {
                "reveal": "always",
                "panel": "new"
            },
            "runOptions": {
                "runOn": "folderOpen"
            }
        }
    ]
}
```

Usage: No action required - the server starts automatically when opening the project in VSCode/Cursor.

#### `.cursor/settings.json`

Cursor-specific settings to run the WebSocket server when opening the project:

```json
{
    "startup": {
        "commands": [
            "powershell -ExecutionPolicy Bypass -WindowStyle Hidden -Command \"Start-Process powershell -ArgumentList '-ExecutionPolicy', 'Bypass', '-File', '\"${workspaceRoot}/scripts/start.ps1\"' -WindowStyle Hidden\""
        ]
    }
}
```

Usage: No action required - the server starts automatically when opening the project in Cursor.

### Figma Plugin

1. In Figma, go to Plugins > Development > Import plugin from manifest
2. Select the `src/cursor_mcp_plugin/manifest.json` file from your project
3. The plugin will appear as "Cursor MCP Plugin" in your development plugins
4. Run the plugin and note the channel ID shown in the plugin window
5. Use this channel ID with the `join_channel` command in Cursor

## Usage

1. Start the WebSocket server
2. Install the MCP server in Cursor
3. Open Figma and run the Cursor MCP Plugin
4. Connect the plugin to the WebSocket server by joining a channel using `join_channel`
5. Use Cursor to communicate with Figma using the MCP tools

## MCP Tools

The MCP server provides the following tools for interacting with Figma:

### Document & Selection

- `get_document_info` - Get information about the current Figma document
- `get_selection` - Get information about the current selection
- `get_node_info` - Get detailed information about a specific node

### Creating Elements

- `create_rectangle` - Create a new rectangle with position, size, and optional name
- `create_frame` - Create a new frame with position, size, and optional name
- `create_text` - Create a new text node with customizable font properties

### Modifying text content

- `set_text_content` - Set the text content of an existing text node

### Styling

- `set_fill_color` - Set the fill color of a node (RGBA)
- `set_stroke_color` - Set the stroke color and weight of a node
- `set_corner_radius` - Set the corner radius of a node with optional per-corner control

### Layout & Organization

- `move_node` - Move a node to a new position
- `resize_node` - Resize a node with new dimensions
- `delete_node` - Delete a node

### Components & Styles

- `get_styles` - Get information about local styles
- `get_local_components` - Get information about local components
- `get_team_components` - Get information about team components
- `create_component_instance` - Create an instance of a component

### Export & Advanced

- `export_node_as_image` - Export a node as an image (PNG, JPG, SVG, or PDF)
- `execute_figma_code` - Execute arbitrary JavaScript code in Figma (use with caution)

### Connection Management

- `join_channel` - Join a specific channel to communicate with Figma

## Development

### Building the Figma Plugin

1. Navigate to the Figma plugin directory:

   ```
   cd src/cursor_mcp_plugin
   ```

2. Edit code.js and ui.html

## Best Practices

When working with the Figma MCP:

1. Always join a channel before sending commands
2. Get document overview using `get_document_info` first
3. Check current selection with `get_selection` before modifications
4. Use appropriate creation tools based on needs:
   - `create_frame` for containers
   - `create_rectangle` for basic shapes
   - `create_text` for text elements
5. Verify changes using `get_node_info`
6. Use component instances when possible for consistency
7. Handle errors appropriately as all commands can throw exceptions

## License

MIT

Added Windows support including:

- PowerShell setup script for Windows installation
- Windows-specific commands for running Bun
- Updated documentation with Windows-specific instructions
- Added Windows-specific MCP configuration
