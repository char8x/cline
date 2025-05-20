# Chat Components Documentation (`webview-ui/src/components/chat`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/chat` directory. Subdirectories like `__tests__` and `auto-approve-menu` are excluded.

## Component Index

*   [`Announcement.tsx`](#announcementtsx)
*   [`BrowserSessionRow.tsx`](#browsersessionrowtsx)
*   [`ChatErrorBoundary.tsx`](#chaterrorboundarytsx)
*   [`ChatRow.tsx`](#chatrowtsx)
*   [`ChatTextArea.tsx`](#chattextareatsx)
*   [`ContextMenu.tsx`](#contextmenutsx)
*   [`CreditLimitError.tsx`](#creditlimiterrortsx)
*   [`NewTaskPreview.tsx`](#newtaskpreviewtsx)
*   [`OptionsButtons.tsx`](#optionsbuttonstsx)
*   [`QuoteButton.tsx`](#quotebuttontsx)
*   [`QuotedMessagePreview.tsx`](#quotedmessagepreviewtsx)
*   [`ReportBugPreview.tsx`](#reportbugpreviewtsx)
*   [`ServersToggleModal.tsx`](#serverstogglemodaltsx)
*   [`SlashCommandMenu.tsx`](#slashcommandmenutsx)
*   [`TaskFeedbackButtons.tsx`](#taskfeedbackbuttonstsx)
*   [`TaskHeader.tsx`](#taskheadertsx)
*   [`TaskTimeline.tsx`](#tasktimelinetsx)
*   [`TaskTimelineTooltip.tsx`](#tasktimelinetooltiptsx)
*   [`UserMessage.tsx`](#usermessagetsx)

---

## `Announcement.tsx`

*   **Primary Functionality**: Displays an announcement or "What's New" block, typically shown after an update. It highlights new features, includes a section for previous updates in an accordion, and provides links to community channels.
*   **Main UI Elements**: Main `div`, close button (`VSCodeButton`), title (`<h3>`), unordered lists (`<ul>`) for features, `<Accordion>` and `<AccordionItem>` (from `@heroui/react`) for previous updates, `<hr>`, and `<VSCodeLink>`s.
*   **Key Props**:
    *   `version: string`: Current extension version.
    *   `hideAnnouncement: () => void`: Callback to hide the announcement.
*   **State Variables**: None (stateless, memoized).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `@/utils/vscStyles`, `@heroui/react`.
*   **Component Type**: Informational / Display Component.
*   **Conditional Display**: Likely shown based on global state comparing `latestAnnouncementId` with `lastAnnouncementShown`. Appears in the main chat view.

---

## `BrowserSessionRow.tsx`

*   **Primary Functionality**: Renders a complex row representing a browser interaction session. Displays browser state (URL, screenshot, console logs, mouse position), actions taken by Cline, pagination for multi-step sessions, and loading indicators.
*   **Main UI Elements**: Main container (`BrowserSessionRowContainer`), browser icon or progress indicator, text indicating browser usage, URL bar, `<BrowserSettingsMenu />`, screenshot area (with `<img>` or globe icon), `<BrowserCursor />`, collapsible console logs section (`<CodeBlock />`), action display area (`BrowserSessionRowContent` which uses `ChatRowContent` or `BrowserActionBox`), pagination buttons (`VSCodeButton`).
*   **Key Props**: `messages`, `isExpanded`, `onToggleExpand`, `lastModifiedMessage`, `isLast`, `onHeightChange`, `onSetQuote`.
*   **State Variables**: `maxActionHeight`, `consoleLogsExpanded`, `currentPageIndex`. Many derived states via `useMemo` for processing messages into pages and determining current display state.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `fast-deep-equal`, `react`, `react-use`, `styled-components`, `@shared/BrowserSettings`, `@shared/ExtensionMessage`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@/components/browser/BrowserSettingsMenu`, `@/components/common/CheckpointControls`, `@/components/common/CodeBlock`, `@/components/chat/ChatRow`.
*   **Component Type**: Data Display Component / Major View Component part.
*   **Conditional Display**: Rendered as a specific row type in `ChatView.tsx` when messages indicate a browser session.

---

## `ChatErrorBoundary.tsx`

*   **Primary Functionality**:
    *   `ChatErrorBoundary`: Catches JavaScript errors in its children, logs them, and displays a fallback UI.
    *   `ErrorAfterDelay`: A demo component to test the error boundary by throwing an error after a delay.
*   **Main UI Elements**:
    *   `ChatErrorBoundary`: Renders children or a styled `div` with error title and body.
    *   `ErrorAfterDelay`: Renders a small `div` indicating it will cause an error with a countdown.
*   **Key Props**:
    *   `ChatErrorBoundary`: `children`, `errorTitle?`, `errorBody?`, `height?`.
    *   `ErrorAfterDelay`: `numSecondsToWait?`.
*   **State Variables**:
    *   `ChatErrorBoundary`: `hasError: boolean`, `error: Error | null`.
    *   `ErrorAfterDelay`: `tickCount: number`.
*   **Direct Code Dependencies**: `react`, `NodeJS` (type-only for `ErrorAfterDelay`).
*   **Component Type**:
    *   `ChatErrorBoundary`: Utility Component / Higher-Order Component (behaviorally).
    *   `ErrorAfterDelay`: Test/Demo Component.
*   **Conditional Display**: `ChatErrorBoundary`'s fallback UI is shown based on its `hasError` state. `ErrorAfterDelay` is for development.

---

## `ChatRow.tsx`

*   **Primary Functionality**:
    *   `ChatRow`: Memoized container for a single chat row, monitors and reports height changes using `useSize` for virtualization.
    *   `ChatRowContent`: Core component rendering diverse chat message content based on message type (text, errors, commands, tool usage, API requests, follow-ups, etc.). Handles icons, text, code blocks, accordions, interactive buttons, copy/quote functionality.
    *   `ProgressIndicator`: Displays a scaled progress ring.
    *   `Markdown`: Renders Markdown text via `<MarkdownBlock />`.
    *   `WithCopyButton`: HOC-like component adding a hover-to-copy button.
*   **Main UI Elements (by `ChatRowContent`)**: Header (icon, title), content area (varies: `<MarkdownBlock />`, `<CodeAccordian />`, `<CodeBlock />`, specific previews like `<NewTaskPreview />`, interactive elements like `<OptionsButtons />`, `<TaskFeedbackButtons />`, `<CheckmarkControl />`). Also `<QuoteButton />`.
*   **Key Props (`ChatRow`)**: `message`, `isExpanded`, `onToggleExpand`, `lastModifiedMessage`, `isLast`, `onHeightChange`, `inputValue?`, `sendMessageFromChatRow?`, `onSetQuote`.
*   **State Variables (`ChatRowContent`)**: `seeNewChangesDisabled`, `quoteButtonState`. (`WithCopyButton` has `copied` state).
*   **Direct Code Dependencies**: Numerous, including `@vscode/webview-ui-toolkit/react`, `fast-deep-equal`, `react`, `react-use`, `styled-components`, shared types/utils, context, gRPC client, many common components, and other chat components.
*   **Component Type**:
    *   `ChatRow`: Layout / Wrapper.
    *   `ChatRowContent`: Major Data Display / UI Control.
    *   Others: Smaller UI Controls / Display.
*   **Conditional Display**: Rendered as rows in `ChatView.tsx`. Content depends on `message` prop. Behavior varies with `isLast`, `lastModifiedMessage`.

---

## `ChatTextArea.tsx`

*   **Primary Functionality**: Primary user input area for chat. Handles text input, @-mentions, /slash commands, image attachments (including drag-and-drop and paste), "Plan"/"Act" mode switching, model selection, and other controls.
*   **Main UI Elements**: Main `div` (drag-and-drop handler), `<SlashCommandMenu />` (conditional), `<ContextMenu />` (conditional for @-mentions), highlight layer `div`, `<DynamicTextArea />` (react-textarea-autosize), `<Thumbnails />` (for selected images), Send button, ControlsContainer (with @-button, camera button, `<ServersToggleModal />`, `<ClineRulesToggleModal />`, model display button with `<ModelSelectorTooltip>` containing `<ApiOptions />`, "Plan"/"Act" switch). Error messages for file issues.
*   **Key Props**: `inputValue`, `activeQuote`, `setInputValue`, `sendingDisabled`, `placeholderText`, `selectedImages`, `setSelectedImages`, `onSend`, `onSelectImages`, `shouldDisableImages`, `onHeightChange?`, `onFocusChange?`.
*   **State Variables**: Numerous states for managing focus, drag state, menus (slash, context, model selector), image handling, cursor position, input highlights, pending insertions, error display, etc.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `react-textarea-autosize`, `react-use`, `styled-components`, shared types/utils, context, gRPC client, common components (`CodeBlock` for BG color, `Thumbnails`, `Tooltip`), settings components (`ApiOptions`), other chat components (`ContextMenu`, `SlashCommandMenu`, `ServersToggleModal`), and `ClineRulesToggleModal`.
*   **Component Type**: Major UI Control / Input Component.
*   **Conditional Display**: Core part of `ChatView.tsx`. Internal elements (menus, tooltips, errors) are conditional on state.

---

## `ContextMenu.tsx`

*   **Primary Functionality**: Renders a context menu for @-mentions in `ChatTextArea`. Displays filterable suggestions (files, folders, Git commits, "Problems", "Terminal") based on user input after "@". Supports keyboard/mouse selection.
*   **Main UI Elements**: Positioned main `div`, scrollable inner `div` (`menuRef`), conditional "Searching..." indicator, list of option `div`s (each with icon and text content via `renderOptionContent`).
*   **Key Props**: `onSelect`, `searchQuery`, `onMouseDown`, `selectedIndex`, `setSelectedIndex`, `selectedType`, `queryItems`, `dynamicSearchResults?`, `isLoading?`.
*   **State Variables**: `showDelayedLoading: boolean`.
*   **Direct Code Dependencies**: `react`, `@/utils/context-mentions`, `@/components/common/CodeAccordian` (for `cleanPathPrefix`).
*   **Component Type**: UI Control.
*   **Conditional Display**: Displayed by `ChatTextArea.tsx` based on its `showContextMenu` state (triggered by "@").

---

## `CreditLimitError.tsx`

*   **Primary Functionality**: Displays an error message when a user hits their credit limit. Shows balance, spending, promotions, and provides "Buy Credits" and "Retry Request" actions.
*   **Main UI Elements**: Main `div`, error message `div`, balance/spent/promotions display `div`s, `<VSCodeButtonLink />` to buy credits, `<VSCodeButton />` to retry.
*   **Key Props**: `currentBalance`, `totalSpent`, `totalPromotions`, `message`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `@/components/common/VSCodeButtonLink`, `@vscode/webview-ui-toolkit/react`, `@/utils/vscode`, `@shared/ExtensionMessage`.
*   **Component Type**: Informational / UI Control.
*   **Conditional Display**: Displayed within `ChatRowContent.tsx` if an API request fails due to credit limits.

---

## `NewTaskPreview.tsx`

*   **Primary Functionality**: Displays a preview of a new task or condensed conversation context, used when Cline suggests these actions.
*   **Main UI Elements**: Main styled `div`, "Task" `span`, `<MarkdownBlock />` for the context.
*   **Key Props**: `context: string`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `@/components/common/MarkdownBlock`.
*   **Component Type**: Display Component.
*   **Conditional Display**: Displayed in `ChatRowContent.tsx` for `new_task` or `condense` `ask` messages.

---

## `OptionsButtons.tsx`

*   **Primary Functionality**: Displays a list of selectable option buttons, typically in response to a question from Cline. Buttons become non-interactive if an option is already selected or if not in an active state.
*   **Main UI Elements**: Main `div`, list of styled `<OptionButton />`s.
*   **Key Props**: `options?`, `selected?`, `isActive?`, `inputValue?`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `styled-components`, `@/components/common/CodeBlock` (for BG color), `@/utils/vscode`, `react`.
*   **Component Type**: UI Control.
*   **Conditional Display**: Displayed in `ChatRowContent.tsx` for `followup` or `plan_mode_respond` `ask` messages that include options.

---

## `QuoteButton.tsx`

*   **Primary Functionality**: Renders a small button near selected text in a chat message. Clicking it triggers quoting the selected text.
*   **Main UI Elements**: `<ButtonContainer>` (styled `div` for positioning/styling), `<VSCodeButton>` with `codicon-quote`.
*   **Key Props**: `top`, `left`, `onClick`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `styled-components`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: UI Control.
*   **Conditional Display**: Displayed by `ChatRowContent.tsx` (via `WithCopyButton`) based on text selection.

---

## `QuotedMessagePreview.tsx`

*   **Primary Functionality**: Displays a preview of a message being quoted in a reply, showing a text snippet and a dismiss button.
*   **Main UI Elements**: `<PreviewContainer>`, `<ContentRow>`, `<ReplyIcon>` (`codicon-reply`), `<TextContainer>` (for truncated text), `<DismissButton>` (`VSCodeButton` with `codicon-close`).
*   **Key Props**: `text`, `onDismiss`, `isFocused?`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `styled-components`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: Display Component / UI Control.
*   **Conditional Display**: Displayed by `ChatView.tsx` when `activeQuote` state is not null, typically above `ChatTextArea.tsx`.

---

## `ReportBugPreview.tsx`

*   **Primary Functionality**: Displays a preview of a bug report Cline is about to create, parsing details from a JSON string.
*   **Main UI Elements**: Main styled `div`, `<h3>` for title, several conditional sections (each with a label `div` and `<MarkdownBlock />` for details like "What Happened?", "Steps to Reproduce", etc.).
*   **Key Props**: `data: string` (JSON string).
*   **State Variables (derived)**: `bugData: object` (parsed JSON).
*   **Direct Code Dependencies**: `react`, `@/components/common/MarkdownBlock`.
*   **Component Type**: Display Component.
*   **Conditional Display**: Displayed in `ChatRowContent.tsx` for `report_bug` `ask` messages.

---

## `ServersToggleModal.tsx`

*   **Primary Functionality**: Renders a button that opens a modal listing MCP servers, allowing users to toggle their status. Includes a link to full MCP settings.
*   **Main UI Elements**: Main `div` (`modalRef`), button `div` (`buttonRef`) with `<Tooltip />` wrapping `<VSCodeButton />` (server icon). Modal (conditional on `isVisible`): outer `div`, arrow `div`, header ("MCP Servers", gear icon `VSCodeButton`), `<ServersToggleList />`.
*   **Key Props**: None.
*   **State Variables**: `isVisible`, `arrowPosition`, `menuPosition`.
*   **Global State Used**: `mcpServers`.
*   **Direct Code Dependencies**: `react`, `react-use`, `@/context/ExtensionStateContext`, `@/hooks/useNavigator`, `@/components/common/CodeBlock` (for BG color), `@/components/mcp/configuration/tabs/installed/ServersToggleList`, `@/utils/vscode`, `@vscode/webview-ui-toolkit/react`, `@/components/common/Tooltip`.
*   **Component Type**: UI Control / Modal.
*   **Conditional Display**: Rendered in `ChatTextArea.tsx`. Modal display based on local `isVisible` state.

---

## `SlashCommandMenu.tsx`

*   **Primary Functionality**: Displays a menu of available slash commands when "/" is typed in `ChatTextArea`. Filters commands based on user query and allows selection.
*   **Main UI Elements**: Outer positioning `div`, inner scrollable `div` (`menuRef`), list of command `div`s (name, description) or "No matching commands found" message.
*   **Key Props**: `onSelect`, `selectedIndex`, `setSelectedIndex`, `onMouseDown`, `query`.
*   **State Variables**: None (derived `filteredCommands`).
*   **Direct Code Dependencies**: `react`, `@/utils/slash-commands`.
*   **Component Type**: UI Control.
*   **Conditional Display**: Displayed by `ChatTextArea.tsx` based on its `showSlashCommandsMenu` state (triggered by "/").

---

## `TaskFeedbackButtons.tsx`

*   **Primary Functionality**: Renders thumbs up/down buttons for feedback on a task's helpfulness. Stores feedback in `localStorage` to prevent multiple submissions and hides if feedback already given or if from history.
*   **Main UI Elements**: `<Container>`, `<ButtonsContainer>`, two `<ButtonWrapper>`s each with a `VSCodeButton` (`codicon-thumbsup`/`codicon-thumbsdown`).
*   **Key Props**: `messageTs`, `isFromHistory?`, `style?`.
*   **State Variables**: `feedback: TaskFeedbackType | null`, `shouldShow: boolean`.
*   **Direct Code Dependencies**: `react`, `styled-components`, `@/utils/vscode`, `@shared/WebviewMessage`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: UI Control.
*   **Conditional Display**: Rendered in `ChatRowContent.tsx` for `completion_result` messages. Hides based on `isFromHistory` or `localStorage` check.

---

## `TaskHeader.tsx`

*   **Primary Functionality**: Displays a header for the current task including description (expandable), token/cache usage, cost, context window visualization, close button, and `<TaskTimeline />`. Shows checkpoint errors.
*   **Main UI Elements**: Main styled `div`, toggleable section (chevron, "Task" label, truncated text, cost, close button). Expanded: full task text ("See more/less"), `<Thumbnails />` (if images), token/cache info, `<CopyButton />`, `<DeleteButton />`, `<TaskTimeline />`, `ContextWindowComponent`, checkpoint error message.
*   **Key Props**: `task`, `tokensIn`, `tokensOut`, `doesModelSupportPromptCache`, `cacheWrites?`, `cacheReads?`, `totalCost`, `lastApiReqTotalTokens?`, `onClose`.
*   **State Variables**: `isTaskExpanded`, `isTextExpanded`, `showSeeMore`.
*   **Global State Used**: `apiConfiguration`, `currentTaskItem`, `checkpointTrackerErrorMessage`, `clineMessages`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `react-use`, `@shared/context-mentions`, `@shared/ExtensionMessage`, `@/context/ExtensionStateContext`, `@/utils/format`, `@/utils/vscode`, `@/components/common/Thumbnails`, `@/components/settings/ApiOptions`, `@/utils/slash-commands`, `@/components/chat/TaskTimeline`, `@/services/grpc-client`, `@/components/common/HeroTooltip`.
*   **Component Type**: Major Display Component / UI Control.
*   **Helper components/functions defined within**: `highlightSlashCommands`, `highlightMentions`, `highlightText`, `CopyButton`, `DeleteButton`.
*   **Conditional Display**: Displayed at the top of `ChatView.tsx` when `currentTaskItem` is active.

---

## `TaskTimeline.tsx`

*   **Primary Functionality**: Displays a horizontal, scrollable timeline of significant events/messages within a task using colored blocks. Uses `react-virtuoso` for efficient rendering. Each block has a tooltip.
*   **Main UI Elements**: Main `div`, `<style>` tag (to hide scrollbar), `<Virtuoso />` component rendering `TimelineBlock`s. Each `TimelineBlock` is a `<TaskTimelineTooltip />` wrapping a colored `div`.
*   **Key Props**: `messages: ClineMessage[]`.
*   **State Variables**: None (derived `taskTimelinePropsMessages`).
*   **Direct Code Dependencies**: `react`, `react-virtuoso`, `@shared/ExtensionMessage`, `@shared/combineApiRequests`, `@shared/combineCommandSequences`, `@/components/chat/TaskTimelineTooltip`, `@/components/chat/colors`.
*   **Component Type**: Data Display Component.
*   **Conditional Display**: Rendered in `TaskHeader.tsx` when expanded. Only renders if `taskTimelinePropsMessages` is not empty.

---

## `TaskTimelineTooltip.tsx`

*   **Primary Functionality**: Wraps a timeline event block with a tooltip (from `@heroui/react`) showing detailed information: message type description, content snippet, and formatted timestamp.
*   **Main UI Elements**: `<Tooltip />` from `@heroui/react`. Tooltip content: colored circle indicator, message description, timestamp, scrollable content snippet.
*   **Key Props**: `message: ClineMessage`, `children: React.ReactNode`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `@shared/ExtensionMessage`, `@/components/chat/colors`, `@heroui/react`.
*   **Component Type**: Display Component / UI Control.
*   **Conditional Display**: Used in `TaskTimeline.tsx` to wrap each event block. Tooltip shown on hover.

---

## `UserMessage.tsx`

*   **Primary Functionality**: Displays a user-sent message. Supports inline editing. If edited, can trigger checkpoint restore (chat or chat+workspace) before resending. Shows attached images.
*   **Main UI Elements**: Main `div`. If editing: `<DynamicTextArea />`, "Restore All" & "Restore Chat" `<RestoreButton />`s. If not editing: `span` with message text (highlighted). `<Thumbnails />` for images.
*   **Key Props**: `text?`, `images?`, `messageTs?`, `sendMessageFromChatRow?`.
*   **State Variables**: `isEditing: boolean`, `editedText: string`.
*   **Global State Used**: `checkpointTrackerErrorMessage`.
*   **Direct Code Dependencies**: `react`, `@/components/common/Thumbnails`, `@/components/chat/TaskHeader` (for `highlightText`), `react-textarea-autosize`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@shared/WebviewMessage`.
*   **Helper component defined within**: `RestoreButton`.
*   **Component Type**: Data Display Component / UI Control.
*   **Conditional Display**: Rendered in `ChatRowContent.tsx` for `user_feedback` messages.
---
