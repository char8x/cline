# History Components Documentation (`webview-ui/src/components/history`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/history` directory.

## Component Index

*   [`HistoryPreview.tsx`](#historypreviewtsx)
*   [`HistoryView.tsx`](#historyviewtsx)

---

## `HistoryPreview.tsx`

*   **Primary Functionality**: Displays a preview of the three most recent tasks from the user's history. Each item shows task date, truncated description, token/cache/cost info. Includes a "View all history" button.
*   **Main UI Elements**: Main `div`, "Recent Tasks" header (`codicon-comment-discussion`). Maps over the first 3 `taskHistory` items to render clickable `div`s (`history-preview-item`) containing: formatted date, optional favorite star (`codicon-star-full`), truncated task description, token/cache/cost details. A "View all history" `VSCodeButton`.
*   **Key Props**:
    *   `showHistoryView: () => void`: Callback to navigate to the full history view.
*   **State Variables**: None.
*   **Global State Used**: `taskHistory` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `react`, `@/services/grpc-client` (TaskServiceClient), `@/utils/format`.
*   **Component Type**: Data Display Component / UI Control.
*   **Conditional Display**: Likely displayed on the main chat/welcome view (e.g., `ChatView.tsx`, `WelcomeView.tsx`) for quick access to recent tasks. Content conditional on `taskHistory`.

---

## `HistoryView.tsx`

*   **Primary Functionality**: Displays the user's full task history with features for searching, sorting (newest, oldest, most expensive, tokens, relevant), and filtering (favorites, current workspace). Allows selection for individual or batch deletion of tasks. Clicking a task opens it.
*   **Main UI Elements**:
    *   Main `div` (full view).
    *   Header: `<h3>` "History", "Done" `VSCodeButton`.
    *   Controls: `<VSCodeTextField>` (search), `<VSCodeRadioGroup>` (sort options), two `<CustomFilterRadio>`s (workspace/favorites filters), "Select All"/"Select None" `<VSCodeButton>`s.
    *   `<Virtuoso />` list for history items. Each item (`div.history-item`) includes:
        *   `<VSCodeCheckbox>` for selection.
        *   Formatted date.
        *   Delete button (`VSCodeButton` with `codicon-trash` & task size).
        *   Favorite toggle button (`VSCodeButton` with `codicon-star-full`/`codicon-star-empty`).
        *   Task description (truncated, highlighted).
        *   Token, cache, cost info.
        *   `<ExportButton />` (custom).
    *   Footer: `<DangerButton />` ("Delete Selected" or "Delete All History" with total size).
*   **Key Props**:
    *   `onDone: () => void`: Callback for the "Done" button.
*   **State Variables**: `searchQuery`, `sortOption`, `lastNonRelevantSort`, `deleteAllDisabled`, `selectedItems`, `showFavoritesOnly`, `showCurrentWorkspaceOnly`, `pendingFavoriteToggles`, `filteredTasks`.
*   **Global State Used**: `taskHistory`, `totalTasksSize`, `filePaths` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `react-virtuoso`, `react`, `fuse.js`, `@/services/grpc-client` (TaskServiceClient), `@/utils/format`, `@shared/ExtensionMessage`, `react-use`, `@/components/common/DangerButton`.
*   **Component Type**: Major View / Data Display Component / UI Control.
*   **Conditional Display**: Displayed when user navigates to the history view.
*   **Helpers defined within**: `CustomFilterRadio`, `ExportButton`, `highlight` (text highlighting utility).
---
