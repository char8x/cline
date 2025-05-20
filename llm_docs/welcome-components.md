# Welcome Components Documentation (`webview-ui/src/components/welcome`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/welcome` directory.

## Component Index

*   [`HomeHeader.tsx`](#homeheadertsx)
*   [`WelcomeView.tsx`](#welcomeviewtsx)

---

## `HomeHeader.tsx`

*   **Primary Functionality**: Displays a header section, typically for a welcome or home screen. It includes the Cline logo and a welcoming question ("What can I do for you?") with an info tooltip providing more details about Cline's capabilities.
*   **Main UI Elements**: Main `div` (flex column). `<ClineLogoVariable />`. Text content `div` with `<h2>` ("What can I do for you?") and `<HeroTooltip />` wrapping an info icon (`codicon-info`) which shows more details on hover.
*   **Key Props**: None.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `@/assets/ClineLogoVariable`, `@/components/common/HeroTooltip`, `react`.
*   **Component Type**: Display Component / Section Header.
*   **Conditional Display**: Part of a larger view, specifically `WelcomeView.tsx` (though the prompt implies it's for a "home screen" which might be different from the initial `WelcomeView` if the user is already set up). It's also used in `ChatView.tsx` when there are no messages.

---

## `WelcomeView.tsx`

*   **Primary Functionality**: Displays the initial welcome screen for new users. Introduces Cline, explains its capabilities, and provides options to sign up ("Get Started for Free") or configure their own API key.
*   **Main UI Elements**: Main `div` (full screen). `<h2>` ("Hi, I'm Cline"). `<ClineLogoWhite />`. Informational paragraphs (`<p>`) with a `<VSCodeLink>` to Claude 3.7 Sonnet. "Get Started for Free" `VSCodeButton`. "Use your own API key" `VSCodeButton` (toggles API options). Conditional section (if `showApiOptions` is true): `<ApiOptions />` (with `showModelOptions={false}`), "Let's go!" `VSCodeButton`.
*   **Key Props**: None.
*   **State Variables**: `apiErrorMessage: string | undefined`, `showApiOptions: boolean`.
*   **Global State Used**: `apiConfiguration` (from `useExtensionState`).
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `@/context/ExtensionStateContext`, `@/utils/validate`, `@/utils/vscode`, `@/components/settings/ApiOptions`, `@/assets/ClineLogoWhite`, `@/services/grpc-client` (AccountServiceClient), `@shared/proto/common`.
*   **Component Type**: Major View / Onboarding Component.
*   **Conditional Display**: Typically displayed on application start if no existing configuration or task is active (e.g., `showWelcome` state in `App.tsx` is true). The API options section is conditional on local `showApiOptions` state.
---
