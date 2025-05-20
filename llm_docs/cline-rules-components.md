# Cline Rules Components Documentation (`webview-ui/src/components/cline-rules`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/cline-rules` directory.

## Component Index

*   [`ClineRulesToggleModal.tsx`](#clinerulestogglemodaltsx)
*   [`NewRuleRow.tsx`](#newrulerowtsx)
*   [`RuleRow.tsx`](#rulerowtsx)
*   [`RulesToggleList.tsx`](#rulestogglelisttsx)

---

## `ClineRulesToggleModal.tsx`

*   **Primary Functionality**: Renders a button (law icon) that toggles a modal window. The modal lists global, local (workspace), cursor, and windsurf Cline rules, allowing users to enable/disable them via `<RulesToggleList />`.
*   **Main UI Elements**: Main `div` (`modalRef`), button `div` (`buttonRef`) with `<Tooltip />` wrapping `<VSCodeButton />` (law icon). Modal (conditional on `isVisible`): outer `div`, arrow `div`, "Cline Rules" header, sections for "Global Rules" and "Workspace Rules", each using `<RulesToggleList />` to display different rule types.
*   **Key Props**: None.
*   **State Variables**: `isVisible: boolean`, `arrowPosition: number`, `menuPosition: number`.
*   **Global State Used**: `globalClineRulesToggles`, `localClineRulesToggles`, `localCursorRulesToggles`, `localWindsurfRulesToggles` (from `useExtensionState`).
*   **Derived State**: `globalRules`, `localRules`, `cursorRules`, `windsurfRules` (formatted arrays for lists).
*   **Direct Code Dependencies**: `react`, `react-use`, `@/context/ExtensionStateContext`, `@/components/common/CodeBlock` (BG color), `@/utils/vscode`, `@vscode/webview-ui-toolkit/react`, `./RulesToggleList`, `@/components/common/Tooltip`.
*   **Component Type**: UI Control / Modal.
*   **Conditional Display**: Rendered in `ChatTextArea.tsx`. Modal visibility based on local `isVisible` state.

---

## `NewRuleRow.tsx`

*   **Primary Functionality**: UI row for creating a new Cline rule file. Expands from placeholder text to an input field for filename entry. Handles validation (allows `.md`, `.txt`, or no extension, defaulting to `.md`) and triggers rule file creation via `FileServiceClient`.
*   **Main UI Elements**: Main `div` (`componentRef`). Inner `div` (changes based on `isExpanded`):
    *   Expanded: `form` with `input` field for filename and submit `VSCodeButton` (`codicon-add`).
    *   Collapsed: Placeholder `span` ("New rule file...") and `VSCodeButton` (`codicon-add`) to expand.
    *   Conditional error message `div`.
*   **Key Props**: `isGlobal: boolean`.
*   **State Variables**: `isExpanded: boolean`, `filename: string`, `error: string | null`.
*   **Direct Code Dependencies**: `react`, `@/utils/vscode`, `@vscode/webview-ui-toolkit/react`, `react-use`, `@/services/grpc-client` (FileServiceClient), `@shared/proto-conversions/file/rule-files-conversion` (CreateRuleFileRequest type).
*   **Component Type**: UI Control / Form Input.
*   **Conditional Display**: Rendered in `RulesToggleList.tsx` if `showNewRule` is true.

---

## `RuleRow.tsx`

*   **Primary Functionality**: Displays a single row for a Cline rule, showing its name, type-based icon (cursor, windsurf, or none), an enable/disable toggle switch, an edit button, and a delete button.
*   **Main UI Elements**: Main `div`. Inner `div` (styled based on `enabled` status): `span` for rule name (prefixed with optional custom SVG icon for cursor/windsurf). Custom toggle switch (`div`s). "Edit" `VSCodeButton` (`codicon-edit`). "Delete" `VSCodeButton` (`codicon-trash`).
*   **Key Props**: `rulePath: string`, `enabled: boolean`, `isGlobal: boolean`, `ruleType: string`, `toggleRule: (rulePath: string, enabled: boolean) => void`.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/services/grpc-client` (FileServiceClient), `@shared/proto-conversions/file/rule-files-conversion` (DeleteRuleFileRequest type), `react`.
*   **Component Type**: Data Display / UI Control.
*   **Conditional Display**: Rendered by `RulesToggleList.tsx` for each rule. Appearance changes based on `enabled` prop.

---

## `RulesToggleList.tsx`

*   **Primary Functionality**: Renders a list of Cline rules using `RuleRow` components. Conditionally displays a `NewRuleRow` for adding new rules and a "No rules found" message.
*   **Main UI Elements**: Main `div`. If rules exist: maps `RuleRow` components, then conditionally `<NewRuleRow />`. If no rules: conditionally "No rules found" message, then conditionally `<NewRuleRow />`.
*   **Key Props**: `rules: [string, boolean][]`, `toggleRule: (rulePath: string, enabled: boolean) => void`, `listGap?: "small" | "medium" | "large"`, `isGlobal: boolean`, `ruleType: string`, `showNewRule: boolean`, `showNoRules: boolean`.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `./NewRuleRow`, `./RuleRow`, `react`.
*   **Component Type**: List Display Component.
*   **Conditional Display**: Used in `ClineRulesToggleModal.tsx` to list different rule categories. Content conditional on `rules` array and props like `showNewRule`, `showNoRules`.
---
