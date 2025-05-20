# MCP Components Documentation (`webview-ui/src/components/mcp`)

This document provides an overview of the React components related to MCP (Multi-purpose Co-pilot Protocol) functionality, found within `webview-ui/src/components/mcp` and its subdirectories (excluding test folders and non-component utility files).

## Component Index

**Chat Display (`webview-ui/src/components/mcp/chat-display`)**
*   [`ImagePreview.tsx`](#imagepreviewtsx)
*   [`LinkPreview.tsx`](#linkpreviewtsx)
*   [`McpResponseDisplay.tsx`](#mcpresponsedisplaytsx)

**Configuration - Add Server (`webview-ui/src/components/mcp/configuration/tabs/add-server`)**
*   [`AddLocalServerForm.tsx`](#addlocalserverformtsx)
*   [`AddRemoteServerForm.tsx`](#addremoteserverformtsx)

**Configuration - Installed Servers (`webview-ui/src/components/mcp/configuration/tabs/installed`)**
*   [`InstalledServersView.tsx`](#installedserversviewtsx)
*   [`ServersToggleList.tsx`](#serverstogglelisttsx)
    *   **Server Row (`webview-ui/src/components/mcp/configuration/tabs/installed/server-row`)**
        *   [`McpResourceRow.tsx`](#mcpresourcerowtsx)
        *   [`McpToolRow.tsx`](#mcptoolrowtsx)
        *   [`ServerRow.tsx`](#serverrowtsx)

**Configuration - Marketplace (`webview-ui/src/components/mcp/configuration/tabs/marketplace`)**
*   [`McpMarketplaceCard.tsx`](#mcpmarketplacecardtsx)
*   [`McpMarketplaceView.tsx`](#mcpmarketplaceviewtsx)
*   [`McpSubmitCard.tsx`](#mcpsubmitcardtsx)

**Configuration - Main View (`webview-ui/src/components/mcp/configuration`)**
*   [`McpConfigurationView.tsx`](#mcpconfigurationviewtsx)

---

## Chat Display (`webview-ui/src/components/mcp/chat-display`)

### `ImagePreview.tsx`

*   **Primary Functionality**:
    *   `ImagePreview`: Class component for loading and displaying an image from a URL, handling loading/error states, content type verification, and aspect ratio adjustments.
    *   `MemoizedImagePreview`: Memoized version of `ImagePreview`.
    *   `ImagePreviewWithErrorBoundary`: Wraps `MemoizedImagePreview` with `ChatErrorBoundary`.
*   **Main UI Elements**:
    *   `ImagePreview`: Conditional rendering: loading spinner and text; error message with "Open in browser" link; or an `<img>`/`<object>` (for SVG) tag for the image (sanitized).
*   **Key Props (`ImagePreview`)**: `url: string`.
*   **State Variables (`ImagePreview`)**: `loading: boolean`, `error: string | null`, `fetchStartTime: number`.
*   **Direct Code Dependencies**: `react`, `@/utils/vscode`, `dompurify`, `./utils/mcpRichUtil`, `@/components/chat/ChatErrorBoundary`.
*   **Component Type**: `ImagePreview`: Data Display/UI Control. `MemoizedImagePreview`, `ImagePreviewWithErrorBoundary`: Wrappers.
*   **Conditional Display**: Used in `McpResponseDisplay.tsx` for image URLs in rich content. Internals conditional on loading/error state.

### `LinkPreview.tsx`

*   **Primary Functionality**:
    *   `LinkPreview`: Class component to fetch and display a preview for a URL by retrieving Open Graph (OG) data. Handles loading/error states.
    *   `MemoizedLinkPreview`: Memoized version of `LinkPreview`.
    *   `LinkPreviewWithErrorBoundary`: Wraps `MemoizedLinkPreview` with `ChatErrorBoundary`.
*   **Main UI Elements**:
    *   `LinkPreview`: Conditional rendering: loading spinner and text; error message with details and "Open in browser" link; or a card with OG image, title, site name, and description.
*   **Key Props (`LinkPreview`)**: `url: string`.
*   **State Variables (`LinkPreview`)**: `loading: boolean`, `error: ErrorType`, `errorMessage: string | null`, `ogData: OpenGraphData | null`, `hasCompletedFetch: boolean`, `fetchStartTime: number`.
*   **Direct Code Dependencies**: `react`, `@/utils/vscode`, `dompurify`, `./utils/mcpRichUtil`, `@/components/chat/ChatErrorBoundary`.
*   **Component Type**: `LinkPreview`: Data Display/UI Control. `MemoizedLinkPreview`, `LinkPreviewWithErrorBoundary`: Wrappers.
*   **Conditional Display**: Used in `McpResponseDisplay.tsx` for non-image URLs in rich content. Internals conditional on loading/error state.

### `McpResponseDisplay.tsx`

*   **Primary Functionality**:
    *   `McpResponseDisplay`: Renders responses from MCP servers. Supports "rich" mode (rendering URLs as `<ImagePreview />` or `<LinkPreview />`) and "plain" text mode, with a toggle.
    *   `McpResponseDisplayWithErrorBoundary`: Wraps `McpResponseDisplay` with `ChatErrorBoundary`.
*   **Main UI Elements**:
    *   `McpResponseDisplay`: `<ResponseContainer>`, `<ResponseHeader>` (title, `<ToggleSwitch>`), content `div` rendering plain text or rich content (text segments, `<ImagePreview />`, `<LinkPreview />`).
*   **Key Props (`McpResponseDisplay`)**: `responseText: string`.
*   **State Variables (`McpResponseDisplay`)**: `isLoading: boolean`, `displayMode: "rich" | "plain"`, `urlMatches: UrlMatch[]`, `error: string | null`, `forceUpdateCounter: number`.
*   **Direct Code Dependencies**: `react`, `./LinkPreview`, `./ImagePreview`, `styled-components`, `@/components/common/CodeBlock` (BG color), `./utils/mcpRichUtil`, `@/components/chat/ChatErrorBoundary`.
*   **Component Type**: `McpResponseDisplay`: Major Data Display/UI Control. `McpResponseDisplayWithErrorBoundary`: Wrapper.
*   **Conditional Display**: Rendered in `ChatRowContent.tsx` for `mcp_server_response` messages. Rich/plain display depends on `displayMode` state.

---

## Configuration - Add Server (`webview-ui/src/components/mcp/configuration/tabs/add-server`)

### `AddLocalServerForm.tsx`

*   **Primary Functionality**: Provides instructions and a button for users to add a local MCP server by directing them to configure `cline_mcp_settings.json`.
*   **Main UI Elements**: `<FormContainer>`, informational text with `<code>` tag and `<VSCodeLink>` to docs, "Open cline_mcp_settings.json" `VSCodeButton`.
*   **Key Props**: `onServerAdded: () => void` (not actively used by button).
*   **State Variables**: None.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/utils/vscode`, `styled-components`, `@/constants`.
*   **Component Type**: Informational / UI Control.
*   **Conditional Display**: Rendered in "Add Server" tab of `McpConfigurationView.tsx` for local server addition.

### `AddRemoteServerForm.tsx`

*   **Primary Functionality**: Form for adding a remote MCP server via name and URL. Handles submission, validation, and communication with backend to add the server. Displays errors or "Connecting..." message.
*   **Main UI Elements**: Main `div`, informational text with `<VSCodeLink>`. `form` with: `<VSCodeTextField>` (Server Name), `<VSCodeTextField>` (Server URL), error message `div`, submit `VSCodeButton`, conditional "Connecting..." message, "Edit Configuration" `VSCodeButton`.
*   **Key Props**: `onServerAdded: () => void`.
*   **State Variables**: `serverName: string`, `serverUrl: string`, `isSubmitting: boolean`, `error: string`, `showConnectingMessage: boolean`.
*   **Global State Used**: `setMcpServers` (from `useExtensionState`).
*   **Direct Code Dependencies**: `react`, `@/utils/vscode`, `@vscode/webview-ui-toolkit/react`, `@/constants`, `@/services/grpc-client` (McpServiceClient), `@shared/proto-conversions/mcp/mcp-server-conversion`, `@/context/ExtensionStateContext`.
*   **Component Type**: Form / UI Control / Data Input.
*   **Conditional Display**: Rendered in "Add Server" (specifically "Remote Servers") tab of `McpConfigurationView.tsx`.

---

## Configuration - Installed Servers (`webview-ui/src/components/mcp/configuration/tabs/installed`)

### `InstalledServersView.tsx`

*   **Primary Functionality**: Displays installed MCP servers. Provides info about MCP, lists servers using `<ServersToggleList />`, and offers buttons to configure servers or access advanced settings.
*   **Main UI Elements**: Main `div`, informational text with `<VSCodeLink>`s. `<ServersToggleList />`. "Configure MCP Servers" `VSCodeButton`, "Advanced MCP Settings" `<VSCodeLink>`.
*   **Key Props**: None.
*   **State Variables**: None.
*   **Global State Used**: `mcpServers` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/utils/vscode`, `@/context/ExtensionStateContext`, `./ServersToggleList`.
*   **Component Type**: View / Section Component.
*   **Conditional Display**: Rendered as the "Installed" tab in `McpConfigurationView.tsx`.

### `ServersToggleList.tsx`

*   **Primary Functionality**: Renders a list of MCP servers using `ServerRow` components. Shows a "No MCP servers installed" message if empty.
*   **Main UI Elements**: Main `div`. If servers exist: maps `ServerRow` components. Else: "No MCP servers installed" message.
*   **Key Props**: `servers: McpServer[]`, `isExpandable: boolean`, `hasTrashIcon: boolean`, `listGap?: "small" | "medium" | "large"`.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `@shared/mcp`, `./server-row/ServerRow`.
*   **Component Type**: List Display Component.
*   **Conditional Display**: Used in `InstalledServersView.tsx` and `ServersToggleModal.tsx`. Content depends on `servers` prop.

---

### Server Row (`webview-ui/src/components/mcp/configuration/tabs/installed/server-row`)

#### `McpResourceRow.tsx`

*   **Primary Functionality**: Displays a single row for an MCP resource or template, showing URI, name/description, and MIME type.
*   **Main UI Elements**: Main `div`. Header `div` (`codicon-symbol-file`, URI). Description `div`. MIME type `div` with `<code>` tag.
*   **Key Props**: `item: McpResource | McpResourceTemplate`.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `@shared/mcp`.
*   **Component Type**: Data Display Component.
*   **Conditional Display**: Used in `ServerRow.tsx` and `ChatRowContent.tsx`.

#### `McpToolRow.tsx`

*   **Primary Functionality**: Displays a single row for an MCP tool, showing name, description, parameters (name, description, required status), and an auto-approve checkbox.
*   **Main UI Elements**: Main `div`. Header `div` (`codicon-symbol-method`, tool name, conditional "Auto-approve" `<VSCodeCheckbox>`). Description `div`. Conditional parameters section (bordered `div` with "Parameters" label, list of parameters with names and descriptions).
*   **Key Props**: `tool: McpTool`, `serverName?: string`.
*   **State Variables**: None.
*   **Global State Used**: `autoApprovalSettings` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@shared/mcp`, `@/utils/vscode`, `@/context/ExtensionStateContext`.
*   **Component Type**: Data Display / UI Control.
*   **Conditional Display**: Used in `ServerRow.tsx` and `ChatRowContent.tsx`. Checkbox conditional on settings.

#### `ServerRow.tsx`

*   **Primary Functionality**: Displays a row for an installed MCP server, showing name, status, and controls to expand for details (tools, resources, timeout), restart, delete, and toggle enabled/disabled state. Handles server errors.
*   **Main UI Elements**: Main `div`. Clickable header `div` (chevron, name, restart/delete `VSCodeButton`s, enable/disable toggle switch, status indicator circle). Conditional error display (message, "Retry", "Delete" buttons). If expanded and no error: `<VSCodePanels>` ("Tools" tab with `<McpToolRow />`s and "Auto-approve all" checkbox; "Resources" tab with `<McpResourceRow />`s), "Request Timeout" `<VSCodeDropdown>`, "Restart Server" `VSCodeButton`, "Delete Server" `<DangerButton />`.
*   **Key Props**: `server: McpServer`, `isExpandable?: boolean`, `hasTrashIcon?: boolean`.
*   **State Variables**: `isExpanded: boolean`, `isDeleting: boolean`, `timeoutValue: string`.
*   **Global State Used**: `mcpMarketplaceCatalog`, `autoApprovalSettings`, `setMcpServers` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@shared/mcp`, `react`, `@/utils/vscode`, `@vscode/webview-ui-toolkit/react`, `@/utils/mcp`, `@/components/common/DangerButton`, `./McpToolRow`, `./McpResourceRow`, `@/context/ExtensionStateContext`, `@/services/grpc-client` (McpServiceClient), `@shared/proto-conversions/mcp/mcp-server-conversion`, `@shared/proto/mcp`.
*   **Component Type**: Data Display / UI Control / Composite Row.
*   **Conditional Display**: Rendered by `ServersToggleList.tsx`. Expanded state managed internally. Error/details display is conditional.

---

## Configuration - Marketplace (`webview-ui/src/components/mcp/configuration/tabs/marketplace`)

### `McpMarketplaceCard.tsx`

*   **Primary Functionality**: Displays a card for an MCP server in the marketplace, showing logo, name, author, GitHub stats, description, category, tags. Provides an "Install" button and links to GitHub.
*   **Main UI Elements**: `<a>` tag (card link). `div` with `<img>` (logo). Content `div`: header (`<h3>` name, `<StyledInstallButton>`), metadata (`<a>` author, stars, downloads, API key icon), description `p`, tags section (category, tags, gradient fade-out).
*   **Key Props**: `item: McpMarketplaceItem`, `installedServers: McpServer[]`.
*   **State Variables**: `isDownloading: boolean`, `isLoading: boolean`.
*   **Derived State**: `isInstalled: boolean`, `githubAuthorUrl: string`.
*   **Direct Code Dependencies**: `react`, `styled-components`, `@shared/mcp`, `react-use`, `@/services/grpc-client` (McpServiceClient).
*   **Component Type**: UI Control / Data Display Card.
*   **Conditional Display**: Rendered by `McpMarketplaceView.tsx`. Install button state changes based on status.

### `McpMarketplaceView.tsx`

*   **Primary Functionality**: Displays the MCP marketplace. Fetches and lists MCP servers, allowing search, category filtering, and sorting. Renders items as `<McpMarketplaceCard />`s. Includes `<McpSubmitCard />`.
*   **Main UI Elements**: Main `div`. Loading `<VSCodeProgressRing />` or error display with "Retry" button. Filter/sort controls: `<VSCodeTextField>` (search), `<VSCodeDropdown>` (category), `<VSCodeRadioGroup>` (sort). List `div`: "No servers found" message or maps `<McpMarketplaceCard />`s. `<McpSubmitCard />`.
*   **Key Props**: None.
*   **State Variables**: `items: McpMarketplaceItem[]`, `isLoading: boolean`, `error: string | null`, `isRefreshing: boolean`, `searchQuery: string`, `selectedCategory: string | null`, `sortBy: "newest" | "stars" | "name" | "downloadCount"`.
*   **Global State Used**: `mcpServers` (from `useExtensionState`).
*   **Derived State**: `categories: string[]`, `filteredItems: McpMarketplaceItem[]`.
*   **Direct Code Dependencies**: `react`, `@vscode/webview-ui-toolkit/react`, `@shared/mcp`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `./McpMarketplaceCard`, `./McpSubmitCard`.
*   **Component Type**: Major View / Section Component.
*   **Conditional Display**: Rendered as the "Marketplace" tab in `McpConfigurationView.tsx`. Content conditional on loading/error/data state.

### `McpSubmitCard.tsx`

*   **Primary Functionality**: Informational card encouraging users to submit MCP servers to the marketplace via a GitHub issue.
*   **Main UI Elements**: Styled main `div`, `codicon-add` icon. Content `div` with `<h3>` ("Submit MCP Server") and `p` (instructions with `<a>` link to GitHub).
*   **Key Props**: None.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `react`.
*   **Component Type**: Informational Card / Call to Action.
*   **Conditional Display**: Rendered at the end of `McpMarketplaceView.tsx`.

---

## Configuration - Main View (`webview-ui/src/components/mcp/configuration`)

### `McpConfigurationView.tsx`

*   **Primary Functionality**: Main view for MCP server management. Uses tabs ("Marketplace", "Remote Servers", "Installed") to switch between different sections.
*   **Main UI Elements**: Main `div`. Header (`<h3>` "MCP Servers", "Done" `VSCodeButton`). Tab container (`<TabButton />`s). Content container conditionally rendering `<McpMarketplaceView />`, `<AddRemoteServerForm />`, or `<InstalledServersView />`.
*   **Key Props**: `onDone: () => void`, `initialTab?: McpViewTab`.
*   **State Variables**: `activeTab: McpViewTab`.
*   **Global State Used**: `mcpMarketplaceEnabled` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `styled-components`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `./tabs/add-server/AddRemoteServerForm`, `./tabs/marketplace/McpMarketplaceView`, `./tabs/installed/InstalledServersView`, `@shared/mcp`.
*   **Component Type**: Major View / Section Component / Navigation Hub.
*   **Conditional Display**: Displayed when user navigates to MCP server configuration. "Marketplace" tab conditional. Content depends on `activeTab`.
*   **Helpers defined within**: `TabButton`.
---
