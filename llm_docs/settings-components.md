# Settings Components Documentation (`webview-ui/src/components/settings`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/settings` directory. Subdirectories like `__tests__` are excluded.

## Component Index

*   [`ApiOptions.tsx`](#apioptionstsx)
*   [`BrowserSettingsSection.tsx`](#browsersettingssectiontsx)
*   [`ClineAccountInfoCard.tsx`](#clineaccountinfocardtsx)
*   [`FeaturedModelCard.tsx`](#featuredmodelcardtsx)
*   [`ModelDescriptionMarkdown.tsx`](#modeldescriptionmarkdowntsx)
*   [`OpenRouterModelPicker.tsx`](#openroutermodelpickertsx)
*   [`RequestyModelPicker.tsx`](#requestymodelpickertsx)
*   [`SettingsView.tsx`](#settingsviewtsx)
*   [`TerminalSettingsSection.tsx`](#terminalsettingssectiontsx)
*   [`ThinkingBudgetSlider.tsx`](#thinkingbudgetslidertsx)

---

## `ApiOptions.tsx`

*   **Primary Functionality**: Provides a comprehensive UI for configuring API settings for various LLM providers (Cline, OpenRouter, Anthropic, OpenAI, Ollama, etc.). Allows selection of provider, API keys, models, and other provider-specific options (base URLs, regions, custom headers, model configurations). Displays model information.
*   **Main UI Elements**: Main `div`, "API Provider" `<VSCodeDropdown>`. Conditionally renders sections based on selected provider, including: `<VSCodeTextField>` for keys/URLs, `<VSCodeCheckbox>` for boolean options, `<VSCodeRadioGroup>` for choices (e.g., AWS auth), other `<VSCodeDropdown>`s for models/regions. Uses `<OpenRouterModelPicker />`, `<RequestyModelPicker />`, `<ClineAccountInfoCard />`, `<ThinkingBudgetSlider />`, and `ModelInfoView` (helper) for specific provider/model details.
*   **Key Props**: `showModelOptions`, `apiErrorMessage?`, `modelIdErrorMessage?`, `isPopup?`, `saveImmediately?`.
*   **State Variables**: `ollamaModels`, `lmStudioModels`, `vsCodeLmModels` (fetched model lists). Booleans for conditional UI sections (e.g., `anthropicBaseUrlSelected`). `isDescriptionExpanded` (for `ModelInfoView`).
*   **Global State Used**: `apiConfiguration`, `setApiConfiguration`, `uriScheme`, `openRouterModels`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `react-use`, `styled-components`, `vscode` (type), `@shared/api`, `@shared/ExtensionMessage`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `@/services/grpc-client`, `@/utils/vscStyles`, common components, other settings components.
*   **Component Type**: Major UI Control / Configuration Section.
*   **Conditional Display**: Primary part of `SettingsView.tsx`. Can be used in popups (e.g., model selector in `ChatTextArea.tsx`).
*   **Helpers defined within**: `OpenRouterBalanceDisplay`, `ModelInfoView`, `ModelInfoSupportsItem`, `normalizeApiConfiguration()`, `getOpenRouterAuthUrl()`, `formatPrice()`, `formatTiers()`.

---

## `BrowserSettingsSection.tsx`

*   **Primary Functionality**: UI section for browser settings: toggling browser tool usage, viewport size, remote browser connections (enable, host, status check, debug relaunch), and Chrome executable path.
*   **Main UI Elements**: Main `div`, "Browser Settings" `<h3>`. Master toggle `<VSCodeCheckbox>`. `<CollapsibleContent>` for detailed settings: viewport `<VSCodeDropdown>`, remote connection `<VSCodeCheckbox>`, `<ConnectionStatusIndicator />` (helper), remote host `<VSCodeTextField>`, "Launch Browser" `<VSCodeButton>`, relaunch result display, Chrome path `<VSCodeTextField>`.
*   **Key Props**: None.
*   **State Variables**: `localChromePath`, `isCheckingConnection`, `connectionStatus`, `relaunchResult`, `debugMode`, `isBundled`, `detectedChromePath`.
*   **Global State Used**: `browserSettings`.
*   **Direct Code Dependencies**: `react`, `@vscode/webview-ui-toolkit/react`, `debounce`, `@shared/BrowserSettings`, `@/context/ExtensionStateContext`, `@/utils/vscode`, `styled-components`, `@/services/grpc-client`.
*   **Component Type**: Configuration Section / UI Control.
*   **Conditional Display**: Rendered in `SettingsView.tsx`. Collapsible content based on master toggle.
*   **Helpers defined within**: `ConnectionStatusIndicator`.

---

## `ClineAccountInfoCard.tsx`

*   **Primary Functionality**: Displays Cline account status. Shows "View Billing & Usage" button if logged in, or "Sign Up with Cline" button if not. Commented-out code suggests previous display of profile details.
*   **Main UI Elements**: Main `div`. Conditional `VSCodeButton` for "View Billing & Usage" or "Sign Up with Cline".
*   **Key Props**: None.
*   **State Variables**: None (user info from context).
*   **Global State Used**: `user` (Firebase), `handleSignOut` (Firebase), `userInfo`, `apiConfiguration.clineApiKey`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `@/context/FirebaseAuthContext`, `@/utils/vscode`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@shared/proto/common`.
*   **Component Type**: Informational / UI Control.
*   **Conditional Display**: Rendered in `ApiOptions.tsx` when "cline" provider is selected. Content conditional on login status.

---

## `FeaturedModelCard.tsx`

*   **Primary Functionality**: Displays a "card" for a featured LLM model, showing model ID, description, and a label (e.g., "Recommended"). Clickable and indicates selection status.
*   **Main UI Elements**: `<CardContainer>` (styled `div`), `<ModelHeader>` (`<ModelName>`, `<Label>`), `<Description>`.
*   **Key Props**: `modelId`, `description`, `onClick`, `isSelected`, `label`.
*   **State Variables**: None (stateless).
*   **Direct Code Dependencies**: `react`, `styled-components`.
*   **Component Type**: UI Control / Display Component.
*   **Conditional Display**: Used in model selection UIs (e.g., `OpenRouterModelPicker.tsx`) to highlight specific models.

---

## `ModelDescriptionMarkdown.tsx`

*   **Primary Functionality**: Renders Markdown for model descriptions, with "See more" functionality to expand/collapse long descriptions (initial display ~3 lines).
*   **Main UI Elements**: `<StyledMarkdown>` (`div`), inner `div`s for text clamping (`textContainerRef`, `textRef`), rendered Markdown content (`reactContent`), conditional "See more" `<VSCodeLink>`.
*   **Key Props**: `markdown?`, `key`, `isExpanded`, `setIsExpanded`, `isPopup?`.
*   **State Variables**: `reactContent`, `showSeeMore`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `react-remark`, `styled-components`, `@/components/common/CodeBlock` (for BG color).
*   **Component Type**: Display Component / UI Control.
*   **Conditional Display**: Used in `ModelInfoView` (in `ApiOptions.tsx`) for model descriptions. Expansion controlled by parent.

---

## `OpenRouterModelPicker.tsx`

*   **Primary Functionality**: UI for selecting models from OpenRouter/Cline. Features searchable text field, dropdown list with search highlighting, favoriting (star icon), featured models display. Shows model details via `ModelInfoView` and `ThinkingBudgetSlider`.
*   **Main UI Elements**: Main `div`, "Model" label, conditional `<FeaturedModelCard />`s. `<DropdownWrapper>`: `<VSCodeTextField>` (search), `<DropdownList>` (conditional, with `<DropdownItem>`s showing model ID and `<StarIcon />`). If model selected: `<ThinkingBudgetSlider />` (conditional), `<ModelInfoView />`. Default informational paragraph if no model.
*   **Key Props**: `isPopup?`.
*   **State Variables**: `searchTerm`, `isDropdownVisible`, `selectedIndex`, `isDescriptionExpanded`.
*   **Global State Used**: `apiConfiguration`, `setApiConfiguration`, `openRouterModels`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `fuse.js`, `react`, `react-remark`, `react-use`, `styled-components`, `@shared/api`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@/utils/vscode`, `@/components/history/HistoryView` (highlight util), `@/components/settings/ApiOptions`, `@/components/common/CodeBlock` (BG color), `@/components/settings/ThinkingBudgetSlider`, `@/components/settings/FeaturedModelCard`.
*   **Component Type**: UI Control / Composite Component.
*   **Conditional Display**: Rendered in `ApiOptions.tsx` for "openrouter" or "cline" providers if `showModelOptions` is true.
*   **Helpers defined within**: `StarIcon`, `ModelDescriptionMarkdown` (duplicate), `OPENROUTER_MODEL_PICKER_Z_INDEX`.

---

## `RequestyModelPicker.tsx`

*   **Primary Functionality**: UI for selecting models for the "Requesty" provider. Features searchable text field, dropdown list. Shows model details via `ModelInfoView` and `ThinkingBudgetSlider`.
*   **Main UI Elements**: Main `div`, "Model" label. `<DropdownWrapper>`: `<VSCodeTextField>` (search), `<DropdownList>` (conditional, with `<DropdownItem>`s). If model selected: `<ThinkingBudgetSlider />` (conditional), `<ModelInfoView />`. Default informational paragraph.
*   **Key Props**: `isPopup?`.
*   **State Variables**: `searchTerm`, `isDropdownVisible`, `selectedIndex`, `isDescriptionExpanded`.
*   **Global State Used**: `apiConfiguration`, `setApiConfiguration`, `requestyModels`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `fuse.js`, `react`, `react-remark`, `react-use`, `styled-components`, `@shared/api`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@/components/history/HistoryView` (highlight util), `@/components/settings/ApiOptions`, `@/components/common/CodeBlock` (BG color), `@/components/settings/ThinkingBudgetSlider`.
*   **Component Type**: UI Control / Composite Component.
*   **Conditional Display**: Rendered in `ApiOptions.tsx` for "requesty" provider if `showModelOptions` is true.
*   **Helpers defined within**: `ModelDescriptionMarkdown` (duplicate), `REQUESTY_MODEL_PICKER_Z_INDEX`.

---

## `SettingsView.tsx`

*   **Primary Functionality**: Main view for all application settings. Configures API options (per mode if enabled), custom instructions, telemetry, browser, and terminal settings. Includes Save button, links to advanced settings/GitHub, and version display.
*   **Main UI Elements**: Main `div`, header ("Settings", "Save" `<VSCodeButton>`). Scrollable area: conditional Plan/Act tabs, `<ApiOptions />`, `<VSCodeTextArea>` (Custom Instructions), checkboxes for "different models for Plan/Act" and telemetry. `<BrowserSettingsSection />`, `<TerminalSettingsSection />`. "Advanced Settings" `<SettingsButton />`. Conditional "Debug" section ("Reset State" button). Footer (GitHub link, version).
*   **Key Props**: `onDone: () => void`.
*   **State Variables**: `apiErrorMessage?`, `modelIdErrorMessage?`, `pendingTabChange`.
*   **Global State Used**: `apiConfiguration`, `version`, `customInstructions`, `setCustomInstructions`, `openRouterModels`, `telemetrySetting`, `setTelemetrySetting`, `chatSettings.mode`, `planActSeparateModelsSetting`, `setPlanActSeparateModelsSetting`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `@/context/ExtensionStateContext`, `@/utils/validate`, `@/utils/vscode`, common/settings components, `@/components/mcp/configuration/McpConfigurationView` (TabButton), `react-use`, `@shared/ExtensionMessage`, `@shared/services/feature-flags/feature-flags`.
*   **Component Type**: Major View / Section Component.
*   **Conditional Display**: Displayed when user navigates to settings. Internal content conditional on various settings (e.g., `planActSeparateModelsSetting`, `IS_DEV`).

---

## `TerminalSettingsSection.tsx`

*   **Primary Functionality**: UI section for terminal settings, specifically shell integration timeout.
*   **Main UI Elements**: Main `div`, "Terminal Settings" `<h3>`. Label, `<VSCodeTextField>` for timeout, conditional error message `div`. Descriptive paragraph.
*   **Key Props**: None.
*   **State Variables**: `inputValue: string`, `inputError: string | null`.
*   **Global State Used**: `shellIntegrationTimeout`, `setShellIntegrationTimeout`.
*   **Direct Code Dependencies**: `react`, `@vscode/webview-ui-toolkit/react`, `@/context/ExtensionStateContext`, `@/utils/vscode`.
*   **Component Type**: Configuration Section / UI Control.
*   **Conditional Display**: Rendered in `SettingsView.tsx`.

---

## `ThinkingBudgetSlider.tsx`

*   **Primary Functionality**: UI for enabling and configuring an "extended thinking budget" in tokens for compatible LLM models.
*   **Main UI Elements**: `<Container>` (`div`), `<VSCodeCheckbox>` ("Enable extended thinking"). Conditional (if enabled): `<LabelContainer>` (`<Label>` for current budget), `<RangeInput>` (slider), `<Description>` (`p`).
*   **Key Props**: `apiConfiguration`, `setApiConfiguration`, `maxBudget?`.
*   **State Variables**: `localValue: number` (for slider interaction).
*   **Derived State**: `isEnabled`, `maxSliderValue`.
*   **Direct Code Dependencies**: `react`, `@shared/api`, `@vscode/webview-ui-toolkit/react`, `styled-components`.
*   **Component Type**: UI Control.
*   **Conditional Display**: Rendered in `ApiOptions.tsx` (and potentially model pickers) when a model supporting this feature is selected.
---
