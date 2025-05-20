# Browser Components Documentation (`webview-ui/src/components/browser`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/browser` directory.

## Component Index

*   [`BrowserSettingsMenu.tsx`](#browsersettingsmenutsx)

---

## `BrowserSettingsMenu.tsx`

*   **Primary Functionality**: Renders a small menu typically intended for display within a browser session's URL bar (e.g., in `BrowserSessionRow.tsx`). It shows an icon indicating the browser's connection status (local/remote, connected/disconnected). Clicking this status icon reveals a popover with more detailed connection information. It also includes a settings gear button to navigate the user to the main browser settings section of the application.
*   **Main UI Elements**:
    *   A main `div` container.
    *   A status icon `VSCodeButton` (using `codicon-remote`, `codicon-vm-running`, or `codicon-info` based on status) that toggles an information popover. The icon's color also changes to reflect the connection status.
    *   An `<InfoPopover>` (styled `div`), conditionally displayed, which shows:
        *   A "Browser Connection" title (`<h4>`).
        *   Rows (`<InfoRow>`) for "Status", "Type" (if connected), and "Remote Host" (if connected and remote).
    *   A settings gear `VSCodeButton` (`codicon-settings-gear`) to open the browser settings view.
*   **Key Props**: None.
*   **State Variables**:
    *   `showInfoPopover: boolean`: Controls visibility of the connection information popover.
    *   `connectionInfo: { isConnected: boolean, isRemote: boolean, host?: string }`: Stores details of the browser connection, updated via gRPC calls.
*   **Global State Used**: `browserSettings` (from `useExtensionState`) for initial state and effect dependencies.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `styled-components`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `@/components/common/CodeBlock` (for `CODE_BLOCK_BG_COLOR`, though not directly used in JSX), `@/services/grpc-client` (BrowserServiceClient).
*   **Component Type**: UI Control / Informational Display.
*   **Conditional Display**: Designed to be embedded in other components (like `BrowserSessionRow.tsx`). The info popover is conditional on the `showInfoPopover` state. Connection status and details are fetched and updated via gRPC and polling.
---
