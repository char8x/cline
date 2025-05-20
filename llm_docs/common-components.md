# Common Components Documentation

This document provides an overview of the reusable React components found in the `webview-ui/src/components/common` directory.

## Component Index

*   [`CheckmarkControl.tsx`](#checkmarkcontroltsx)
*   [`CheckpointControls.tsx`](#checkpointcontrolstsx)
*   [`CodeAccordian.tsx`](#codeaccordiantsx)
*   [`CodeBlock.tsx`](#codeblocktsx)
*   [`DangerButton.tsx`](#dangerbuttontsx)
*   [`Demo.tsx`](#demotsx)
*   [`HeroTooltip.tsx`](#herotooltiptsx)
*   [`MarkdownBlock.tsx`](#markdownblocktsx)
*   [`MermaidBlock.tsx`](#mermaidblocktsx)
*   [`SettingsButton.tsx`](#settingsbuttontsx)
*   [`SuccessButton.tsx`](#successbuttontsx)
*   [`TelemetryBanner.tsx`](#telemetrybannertsx)
*   [`Thumbnails.tsx`](#thumbnailstsx)
*   [`Tooltip.tsx`](#tooltiptsx)
*   [`VSCodeButtonLink.tsx`](#vscodebuttonlinktsx)

---

## `CheckmarkControl.tsx`

*   **Primary Functionality**: Renders a "checkpoint" indicator within a message, providing controls to interact with that checkpoint. It allows users to compare the current state with the checkpoint, and restore the task, workspace, or both to the state at the checkpoint. The visual appearance changes if the checkpoint is the currently restored one.
*   **Main UI Elements**:
    *   An icon (`codicon-bookmark`).
    *   A label: "Checkpoint" or "Checkpoint (restored)".
    *   Dotted lines for visual separation.
    *   A "Compare" button.
    *   A "Restore" button, which when clicked, reveals a tooltip/popover (`RestoreConfirmTooltip`) rendered via `createPortal`.
    *   The `RestoreConfirmTooltip` contains:
        *   "Restore Files" button with a description.
        *   "Restore Task Only" button with a description.
        *   "Restore Files & Task" button with a description.
*   **Key Props**:
    *   `messageTs?: number`: Timestamp or identifier for the message/checkpoint.
    *   `isCheckpointCheckedOut?: boolean`: Indicates if this checkpoint is the currently active/restored one.
*   **Key State Variables**:
    *   `compareDisabled: boolean`: Disables the "Compare" button during async operations.
    *   `restoreTaskDisabled: boolean`: Disables the "Restore Task Only" button.
    *   `restoreWorkspaceDisabled: boolean`: Disables the "Restore Files" button.
    *   `restoreBothDisabled: boolean`: Disables the "Restore Files & Task" button.
    *   `showRestoreConfirm: boolean`: Controls the visibility of the restore options tooltip.
    *   `hasMouseEntered: boolean`: Tracks mouse hover state for the restore tooltip.
    *   Tooltip positioning state from `@floating-ui/react`.
*   **Direct Code Dependencies**: `react`, `react-use`, `styled-components`, `@shared/ExtensionMessage`, `@shared/WebviewMessage`, `@/services/grpc-client`, `@/utils/vscode`, `@/components/common/CodeBlock` (for `CODE_BLOCK_BG_COLOR`), `@vscode/webview-ui-toolkit/react`, `react-dom`, `@floating-ui/react`.
*   **Component Type**: UI Control.

---

## `CheckpointControls.tsx`

*   **Primary Functionality**: Provides an overlay with controls for managing a checkpoint, typically appearing on hover over a message or element that has an associated checkpoint. It allows users to compare the current state with the checkpoint and restore the task, workspace, or both.
*   **Main UI Elements**:
    *   A "Compare" button (`VSCodeButton`) with a `codicon-diff-multiple` icon.
    *   A "Restore" button (`VSCodeButton`) with a `codicon-discard` icon, which upon click reveals a `RestoreConfirmTooltip`.
    *   The `RestoreConfirmTooltip` (conditionally rendered) contains:
        *   "Restore Task and Workspace" button with a description.
        *   "Restore Task Only" button with a description.
        *   "Restore Workspace Only" button with a description.
*   **Key Props**:
    *   `messageTs?: number`: Timestamp or identifier for the message/checkpoint.
*   **Key State Variables**:
    *   `compareDisabled: boolean`: Disables the "Compare" button.
    *   `restoreTaskDisabled: boolean`: Disables the "Restore Task Only" button.
    *   `restoreWorkspaceDisabled: boolean`: Disables the "Restore Workspace Only" button.
    *   `restoreBothDisabled: boolean`: Disables the "Restore Task and Workspace" button.
    *   `showRestoreConfirm: boolean`: Controls visibility of the restore options tooltip.
    *   `hasMouseEntered: boolean`: Tracks mouse hover state for the tooltip.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `react-use`, `styled-components`, `@shared/ExtensionMessage`, `@/services/grpc-client`, `@/utils/vscode`, `@/components/common/CodeBlock` (for `CODE_BLOCK_BG_COLOR`), `@shared/WebviewMessage`.
*   **Component Type**: UI Control.

---

## `CodeAccordian.tsx`

*   **Primary Functionality**: Displays a collapsible code block. It can show regular code or a diff. Features a header that can display a file path (with leading ellipsis for long paths), "User Edits" (for feedback), or "Console Logs". The header also shows the number of edits if applicable and an expand/collapse chevron.
*   **Main UI Elements**:
    *   A main `div` container for the accordion.
    *   A header `div` (if `path`, `isFeedback`, or `isConsoleLogs` is true) containing:
        *   Icon (`codicon-feedback` or `codicon-output`) and label ("User Edits" or "Console Logs").
        *   File path (potentially truncated).
        *   Edit count (`numberOfEdits`) with `codicon-diff-single`.
        *   Expand/collapse chevron icon.
    *   A `div` wrapping the `<CodeBlock />` component (displayed if expanded or no header).
*   **Key Props**:
    *   `code?: string`: Code content.
    *   `diff?: string`: Diff content.
    *   `language?: string`: Language of the code.
    *   `path?: string`: File path.
    *   `isFeedback?: boolean`: For "User Edits" header.
    *   `isConsoleLogs?: boolean`: For "Console Logs" header.
    *   `isExpanded: boolean`: Controls expansion state.
    *   `onToggleExpand: () => void`: Callback for header click.
    *   `isLoading?: boolean`: Loading state indication.
*   **Derived State (via `useMemo`)**:
    *   `inferredLanguage`: Determined from `language` or `path`.
    *   `numberOfEdits`: Calculated from `code` content.
*   **Direct Code Dependencies**: `react`, `@/utils/getLanguageFromPath`, `@/components/common/CodeBlock`.
*   **Component Type**: UI Control / Display Component.
*   **Exports**: Also exports `cleanPathPrefix` utility function.

---

## `CodeBlock.tsx`

*   **Primary Functionality**: Renders a block of source code with syntax highlighting. It uses `react-remark` and `rehype-highlight` to process Markdown-formatted code and apply styling. Handles theming for syntax highlighting based on `ExtensionStateContext` and supports forced text wrapping.
*   **Main UI Elements**:
    *   A top-level `div` for overflow and background.
    *   `<StyledMarkdown>`: A styled `div` containing HTML from Markdown (typically `<pre>` with `<code>`).
    *   `<StyledPre>`: Applies theme-specific syntax highlighting.
*   **Key Props**:
    *   `source?: string`: Markdown string with the code block.
    *   `forceWrap?: boolean`: If `true`, forces code wrapping.
*   **State from Hooks**:
    *   `reactContent`: React elements from `source` Markdown.
    *   `setMarkdownSource`: Function to update Markdown source for `useRemark`.
    *   `theme`: Theme styles from `ExtensionStateContext`.
*   **Direct Code Dependencies**: `react`, `react-remark`, `rehype-highlight`, `styled-components`, `unist-util-visit`, `@/context/ExtensionStateContext`.
*   **Component Type**: Display Component / UI Control.
*   **Exports**: Also exports `CODE_BLOCK_BG_COLOR` constant.

---

## `DangerButton.tsx`

*   **Primary Functionality**: Creates a `VSCodeButton` styled to indicate a "danger" or potentially destructive action. It uses reddish hues for background colors in normal, hover, and active states.
*   **Main UI Elements**: Renders a single `<VSCodeButton />`.
*   **Key Props**: Accepts all props of the standard `VSCodeButton`. `className` can be used for additional styling.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`.
*   **Component Type**: UI Control.

---

## `Demo.tsx`

*   **Primary Functionality**: Serves as a demonstration or showcase of various UI components available from the `@vscode/webview-ui-toolkit/react` library. Likely used for development, testing, or as a reference.
*   **Main UI Elements**: Includes a wide array of `VSCode*` components like `VSCodeButton`, `VSCodeDataGrid`, `VSCodeTextField`, `VSCodeProgressRing`, `VSCodeBadge`, `VSCodeCheckbox`, `VSCodeDivider`, `VSCodeDropdown`, `VSCodeLink`, `VSCodePanels`, `VSCodeRadioGroup`, `VSCodeTag`, and `VSCodeTextArea`.
*   **Key Props**: None for the `Demo` component itself.
*   **State Variables**: None.
*   **Direct Code Dependencies**: Numerous components from `@vscode/webview-ui-toolkit/react`, `react`.
*   **Component Type**: Demo/Showcase Component.

---

## `HeroTooltip.tsx`

*   **Primary Functionality**: A wrapper around the `Tooltip` component from `@heroui/react`. It standardizes the appearance (custom styling) and behavior (default delays) of tooltips within the application.
*   **Main UI Elements**: Renders the `<Tooltip />` from `@heroui/react`. If `content` is a string, it's wrapped in a styled `div`.
*   **Key Props**:
    *   `content: React.ReactNode`: Content for the tooltip (string or React node).
    *   `children: React.ReactNode`: Element(s) the tooltip is attached to.
    *   `className?: string`: Optional CSS class for the styled `div` (if content is string).
    *   `delay?: number`: Appear delay (default `0`).
    *   `closeDelay?: number`: Disappear delay (default `500`, but overridden to `0` in usage).
    *   `placement?: "top" | "bottom" | "left" | "right"`: Tooltip position (default `"top"`).
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `@heroui/react`.
*   **Component Type**: UI Control / Wrapper Component.

---

## `MarkdownBlock.tsx`

*   **Primary Functionality**: Renders Markdown content into HTML. Supports standard Markdown, syntax-highlighted code blocks (with a copy button), LaTeX mathematical expressions (via KaTeX), and Mermaid diagrams. Includes custom remark plugins for auto-linking URLs and preventing misinterpretation of filenames like `__init__.py`.
*   **Main UI Elements**:
    *   `<StyledMarkdown>`: Main container for rendered HTML.
    *   Standard HTML elements (paragraphs, lists, links).
    *   Code blocks (`<pre><code>`) wrapped by `PreWithCopyButton` (which includes a `<CopyButton>`).
    *   Inline code.
    *   Mathematical expressions (KaTeX).
    *   Mermaid diagrams via `<MermaidBlock />`.
*   **Key Props**:
    *   `markdown?: string`: The Markdown string to render.
*   **State from Hooks/Internal**:
    *   `reactContent`: Rendered React elements.
    *   `theme`: Syntax highlighting theme from `ExtensionStateContext`.
    *   `copied` (in `PreWithCopyButton`): Tracks copy button state.
*   **Direct Code Dependencies**: `react`, `react-remark`, `rehype-highlight`, `rehype-katex`, `remark-math`, `styled-components`, `unist-util-visit`, `@/context/ExtensionStateContext`, `@/components/common/CodeBlock` (for `CODE_BLOCK_BG_COLOR`), `@/components/common/MermaidBlock`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: Display Component.

---

## `MermaidBlock.tsx`

*   **Primary Functionality**: Renders diagrams from Mermaid diagramming syntax. Initializes Mermaid with a custom theme, parses the `code`, and renders it as an SVG. Features include a loading state, debounced rendering, a button to copy Mermaid code, and functionality to convert and open the diagram as a PNG on click.
*   **Main UI Elements**:
    *   `<MermaidBlockContainer>`: Main wrapper.
    *   `<LoadingMessage>`: Conditional loading indicator.
    *   `<ButtonContainer>` with `<StyledVSCodeButton>` (copy icon).
    *   `<SvgContainer>`: Where the Mermaid SVG is rendered; clickable for PNG export.
*   **Key Props**:
    *   `code: string`: Mermaid diagram syntax.
*   **Key State Variables**:
    *   `isLoading: boolean`: Tracks parsing/rendering state.
*   **Direct Code Dependencies**: `react`, `mermaid`, `@/utils/useDebounceEffect`, `styled-components`, `@/services/grpc-client`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: Display Component / UI Control.
*   **Exports**: Contains `svgToPng` utility function.

---

## `SettingsButton.tsx`

*   **Primary Functionality**: Creates a specialized `VSCodeButton` styled for "settings" related actions. It uses secondary button colors from the VS Code theme, spans full width, and standardizes `codicon` styling if used as a child.
*   **Main UI Elements**: Renders a single `<StyledButton />` (a styled `VSCodeButton`).
*   **Key Props**: Accepts all props of the standard `VSCodeButton`. Forces `appearance="secondary"`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `styled-components`, `react`.
*   **Component Type**: UI Control.

---

## `SuccessButton.tsx`

*   **Primary Functionality**: Creates a `VSCodeButton` (named `SuccessButtonTW`) styled to indicate a "success" or affirmative action. It uses green hues for background colors.
*   **Main UI Elements**: Renders a single `<VSCodeButton />`.
*   **Key Props**: Accepts all props of the standard `VSCodeButton`. `className` can be used for additional styling.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`.
*   **Component Type**: UI Control.

---

## `TelemetryBanner.tsx`

*   **Primary Functionality**: Renders a banner to inform users about telemetry data collection. It explains what data is (and isn't) collected, provides a link to settings, and includes a close button that implicitly enables telemetry.
*   **Main UI Elements**:
    *   `<BannerContainer>`: Main banner area.
    *   `<CloseButton>`: Dismisses the banner (and enables telemetry).
    *   Text content explaining telemetry.
    *   `<VSCodeLink>` for the "settings" link.
*   **Key Props**: None.
*   **State Variables**: None (stateless, memoized).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `styled-components`, `@/utils/vscode`, `@shared/TelemetrySetting`.
*   **Component Type**: UI Control / Informational Component.

---

## `Thumbnails.tsx`

*   **Primary Functionality**: Displays a list of images as small, clickable thumbnails. Supports optional deletion of thumbnails (via a button appearing on hover if `setImages` prop is provided). Clicking a thumbnail opens the image using `FileServiceClient`. Reports its height changes via `onHeightChange`.
*   **Main UI Elements**:
    *   A main `div` container (flexbox for layout).
    *   For each image: an `<img>` thumbnail and a conditional delete `div` with `codicon-close`.
*   **Key Props**:
    *   `images: string[]`: Array of image data URLs/paths.
    *   `style?: React.CSSProperties`: Styles for the main container.
    *   `setImages?: React.Dispatch<React.SetStateAction<string[]>>`: Enables delete functionality.
    *   `onHeightChange?: (height: number) => void`: Callback for height changes.
*   **Key State Variables**:
    *   `hoveredIndex: number | null`: Index of the hovered thumbnail.
*   **Direct Code Dependencies**: `react`, `react-use`, `@/services/grpc-client`, `@/utils/vscode`.
*   **Component Type**: UI Control / Display Component.

---

## `Tooltip.tsx`

*   **Primary Functionality**: Provides a simple tooltip. When its `children` are hovered (or `visible` prop is true), it displays a `TooltipBody` with `tipText` and optional `hintText`.
*   **Main UI Elements**:
    *   A `div` wrapper for hover detection.
    *   The `children` prop.
    *   Conditionally, `<TooltipBody>` (styled `div`) containing the tip and optional hint.
*   **Key Props**:
    *   `visible?: boolean`: External control for visibility.
    *   `hintText?: string`: Optional additional text.
    *   `tipText: string`: Main tooltip text.
    *   `children: React.ReactNode`: Element(s) triggering the tooltip.
    *   `style?: React.CSSProperties`: Styles for `TooltipBody`.
*   **Key State Variables**:
    *   `isHovered: boolean`: Tracks hover state.
*   **Direct Code Dependencies**: `react`, `styled-components`, `@/utils/vscStyles`.
*   **Component Type**: UI Control / Display Component.

---

## `VSCodeButtonLink.tsx`

*   **Primary Functionality**: Combines the appearance of a `VSCodeButton` with the functionality of an HTML anchor (`<a>`) tag, allowing navigation on click.
*   **Main UI Elements**: An `<a>` tag wrapping a `<VSCodeButton />`. Inline styles on the anchor remove text decoration and inherit color.
*   **Key Props**:
    *   `href: string`: The URL for navigation.
    *   `children: React.ReactNode`: Content for the button.
    *   Accepts any other props for `VSCodeButton`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `@vscode/webview-ui-toolkit/react`.
*   **Component Type**: UI Control.
---
