# Superdesign MCP Server

An MCP (Model Context Protocol) server that brings [Superdesign](https://github.com/superdesigndev/superdesign) - an open source AI design agent by [@jasonzhou1993](https://twitter.com/jasonzhou1993) and [@jackjack_eth](https://twitter.com/jackjack_eth) - to Claude Code as native tools. This server operates as a "design orchestrator" that provides structured specifications for your IDE's LLM to execute, enabling Superdesign's sophisticated design capabilities without requiring Anthropic API keys.

<a href="https://glama.ai/mcp/servers/@jonthebeef/superdesign-mcp-claude-code">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@jonthebeef/superdesign-mcp-claude-code/badge" alt="Superdesign Server MCP server" />
</a>

## Key Benefits

- **No API Keys Required**: Works directly with Claude Code's built-in LLM connection
- **Local Execution**: Runs entirely on your machine as an MCP server
- **IDE Integration**: Seamlessly integrates with Claude Code (and potentially Cursor, Windsurf, or other MCP-compatible IDEs - untested)
- **Based on Open Source**: Built on top of [Superdesign.dev](https://www.superdesign.dev), an open source AI design system

## Installation

1. Install dependencies:
```bash
npm install
```

2. Build the server:
```bash
npm run build
```

## Claude Code Integration

1. Add the MCP server to your Claude Code configuration:
```bash
# Create or edit your Claude Code MCP settings file
# On macOS:
mkdir -p ~/.claude-code
cp claude-mcp-config.json ~/.claude-code/mcp-settings.json

# Or add manually to your existing mcp-settings.json:
```

Example `~/.claude-code/mcp-settings.json`:
```json
{
  "mcpServers": {
    "superdesign": {
      "command": "node",
      "args": ["/path/to/superdesign/dist/index.js"],
      "env": {}
    }
  }
}
```

2. Restart Claude Code

3. The MCP server will provide these Superdesign orchestrator tools in Claude Code:
   - `superdesign_generate` - Returns specifications for Claude to generate designs
   - `superdesign_iterate` - Returns instructions for Claude to iterate on existing designs
   - `superdesign_extract_system` - Returns instructions for design system extraction
   - `superdesign_list` - Lists all created designs in the workspace

## Development

Run in development mode:
```bash
npm run dev
```

## Superdesign Tools Available

### superdesign_generate
Returns structured specifications for Claude Code to generate designs:
- **UI designs**: Complete responsive interfaces
- **Wireframes**: Minimal black and white layouts  
- **Components**: Individual UI components (HTML/React/Vue)
- **Logos**: SVG logo designs
- **Icons**: SVG icon designs

Parameters:
- `prompt`: Description of what to create
- `design_type`: Type of design (ui, wireframe, component, logo, icon)
- `variations`: Number of variations to generate (1-5, default 3)
- `framework`: Framework for components (html, react, vue)

**Output**: Detailed specifications including Superdesign system prompt, file naming patterns, and design guidelines for Claude Code to execute.

### superdesign_iterate
Returns iteration instructions for Claude Code to improve existing designs:
- Reads existing design files  
- Provides structured feedback application guidelines
- Maintains design consistency through Superdesign principles

Parameters:
- `design_file`: Path to existing design file
- `feedback`: Improvement instructions
- `variations`: Number of variations to create

**Output**: Iteration specifications including original design content, feedback to apply, and Superdesign guidelines for Claude Code to execute.

### superdesign_extract_system
Returns instructions for Claude Code to extract design systems from screenshots:
- Provides analysis framework for visual design patterns
- Guides extraction of color palettes, typography, spacing
- Specifies JSON structure for reusable design systems

**Output**: Extraction specifications and JSON schema for Claude Code to analyze images and create design system files.

### superdesign_list
List all Superdesign creations in workspace:
- Shows design iterations
- Shows extracted design systems
- Displays file organization

### superdesign_gallery
Generate an interactive HTML gallery to view all designs:
- **Browser-based gallery** - opens in your default browser
- **Visual previews** - see design thumbnails in a responsive grid
- **Interactive features** - click to view full-screen, copy paths
- **Mobile responsive** - works on desktop, tablet, and mobile
- **Auto-discovery** - finds all HTML/SVG files in design_iterations/

Parameters:
- `workspace_path`: Workspace directory (optional, defaults to current directory)

**Output**: Gallery HTML file with embedded previews and JavaScript interactions. The gallery opens automatically in your browser, providing a Superdesign-like canvas experience.

## How the Orchestrator Works

This MCP server operates as a **design orchestrator** rather than a direct generator:

1. **User Request**: "Generate a modern dashboard UI"
2. **MCP Server**: Returns detailed specifications with:
   - Complete Superdesign system prompt and guidelines
   - Exact file paths and naming conventions
   - Design type-specific instructions
   - Number of variations to create
3. **Claude Code**: Receives specifications and:
   - Generates the actual HTML/SVG/React code
   - Saves files to specified locations
   - Follows all Superdesign design principles

## Usage in Claude Code

Once configured, you can use Superdesign through Claude Code with natural language:

**Example Usage:**
- "Generate a modern dashboard UI design"
- "Create 3 variations of a login page wireframe"  
- "Design a React component for a product card"
- "Make a minimalist logo for a tech startup"
- "Iterate on the dashboard design with better spacing"
- "Show me the gallery of all my designs"

**Requirements:**
- Claude Code with MCP support
- No API keys needed (uses Claude Code's existing LLM connection)

**File Organization:**
Designs are automatically saved to `superdesign/` directory (visible folder):
- `design_iterations/` - Generated designs (HTML/SVG files)
- `design_system/` - Extracted design systems (JSON files)

**Benefits:**
- No API key configuration required
- Uses Claude Code's existing LLM capabilities
- Maintains all of Superdesign's sophisticated design methodology
- Proper file organization and naming conventions
- Full design iteration workflow support

## Known Issues & Troubleshooting

### File Permissions Error
If you encounter permission errors when running the MCP server:
```bash
# Add execute permissions to the built file
chmod +x dist/index.js
```

### MCP Tools Not Appearing
If Superdesign tools don't appear in Claude Code after installation:
1. Ensure you've completely quit Claude Code (not just closed the window)
2. Restart Claude Code from your terminal
3. Verify the server is registered: `claude mcp list`
4. Check the tools are available by asking Claude: "What tools do you have available?"

### Server Registration Issues
If the server fails to register:
```bash
# Remove and re-add the server
claude mcp remove superdesign -s user
claude mcp add --scope user superdesign /path/to/superdesign/dist/index.js
```

### Build Errors
Ensure you have Node.js 16+ installed:
```bash
node --version  # Should be v16.0.0 or higher
```

## 🤝 Relationship to Superdesign

This MCP server provides a complementary integration for [Superdesign](https://github.com/superdesigndev/superdesign) by [@jasonzhou1993](https://twitter.com/jasonzhou1993) and [@jackjack_eth](https://twitter.com/jackjack_eth).

While Superdesign offers an IDE extension that works across multiple editors, this MCP server specifically enhances Claude Code by:
- **Eliminating the need for API keys** - uses Claude Code's built-in LLM connection
- **Providing native tool integration** - no manual prompt copying required
- **Enabling direct tool calls** - seamless workflow without copy/paste

This addresses the community request for Claude Code API provider support (see [Superdesign Issue #3](https://github.com/superdesigndev/superdesign/issues)).

### How it's Different

| Aspect | Superdesign Extension | This MCP Server |
|--------|----------------------|-----------------|
| **Integration Type** | IDE Extension | Native MCP Tools |
| **Claude Code Access** | Manual prompt copying | Direct tool invocation |
| **API Requirements** | Separate API key needed | Uses Claude Code's existing connection |
| **User Experience** | Copy/paste workflow | Automated orchestration |

## License

This project follows the same license as the original Superdesign project. Please refer to the [Superdesign repository](https://www.superdesign.dev) for license details.