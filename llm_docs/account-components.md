# Account Components Documentation (`webview-ui/src/components/account`)

This document provides an overview of the React components found directly within the `webview-ui/src/components/account` directory.

## Component Index

*   [`AccountOptions.tsx`](#accountoptionstsx)
*   [`AccountView.tsx`](#accountviewtsx)
*   [`CreditsHistoryTable.tsx`](#creditshistorytabletsx)

---

## `AccountOptions.tsx`

*   **Primary Functionality**: Triggers the account login process (`AccountServiceClient.accountLoginClicked()`) immediately upon mounting. It does not render any visible UI.
*   **Main UI Elements**: `null` (renders nothing).
*   **Key Props**: None.
*   **State Variables**: None.
*   **Direct Code Dependencies**: `react` (memo), `@/services/grpc-client` (AccountServiceClient), `@shared/proto/common` (EmptyRequest).
*   **Component Type**: Functional / Service-Triggering Component.
*   **Conditional Display**: Likely rendered conditionally by a parent component (e.g., `AccountView`) when it needs to initiate the login flow.

---

## `AccountView.tsx`

*   **Primary Functionality**:
    *   `AccountView`: Main wrapper for the account management screen. Includes a header with a "Done" button.
    *   `ClineAccountView`: Core component displaying user account details or a login/signup prompt. If logged in, shows profile info (avatar, name, email), dashboard/logout buttons, credit balance (with refresh), "Add Credits" button, and `<CreditsHistoryTable />`. If not logged in, shows signup prompt, logo, and links (ToS, Privacy).
*   **Main UI Elements**:
    *   `AccountView`: Main `div`, header (`<h3>` "Account", "Done" `<VSCodeButton>`), container for `<ClineAccountView />`.
    *   `ClineAccountView`:
        *   Logged In: Profile section (`<img>`/placeholder, name, email), "Dashboard" `<VSCodeButtonLink>`, "Log out" `<VSCodeButton>`, `<VSCodeDivider />`, balance section (`<CountUp />`, refresh button), "Add Credits" `<VSCodeButtonLink>`, `<VSCodeDivider />`, `<CreditsHistoryTable />`.
        *   Logged Out: `<ClineLogoWhite />`, signup paragraph, "Sign up" `<VSCodeButton>`, ToS/Privacy `<VSCodeLink>`s.
*   **Key Props**:
    *   `AccountView`: `onDone: () => void`.
    *   `ClineAccountView`: None.
*   **State Variables (`ClineAccountView`)**: `balance: number`, `isLoading: boolean`, `usageData: UsageTransaction[]`, `paymentsData: PaymentTransaction[]`.
*   **Global State Used (`ClineAccountView`)**: `user` (Firebase), `handleSignOut` (Firebase), `userInfo`, `apiConfiguration.clineApiKey`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `@/context/FirebaseAuthContext`, `@/utils/vscode`, `@/components/common/VSCodeButtonLink`, `@/assets/ClineLogoWhite`, `react-countup`, `./CreditsHistoryTable`, `@shared/ClineAccount`, `@/context/ExtensionStateContext`, `@/services/grpc-client`, `@shared/proto/common`.
*   **Component Type**:
    *   `AccountView`: Major View / Section Component.
    *   `ClineAccountView`: Major Data Display / UI Control section.
*   **Conditional Display**: `AccountView` is shown when navigating to account management. `ClineAccountView`'s content depends on login status.

---

## `CreditsHistoryTable.tsx`

*   **Primary Functionality**: Displays tables for credit usage history and payment history, with tabs to switch between them.
*   **Main UI Elements**: Main `div`, `<TabButton>`s for "USAGE HISTORY" and "PAYMENTS HISTORY". Conditional content: "Loading..." message, or a `<VSCodeDataGrid>` for either usage (Date, Model, Credits Used) or payments (Date, Total Cost, Credits). Shows "No history" message if data is empty.
*   **Key Props**: `isLoading: boolean`, `usageData: UsageTransaction[]`, `paymentsData: PaymentTransaction[]`.
*   **State Variables**: `activeTab: "usage" | "payments"`.
*   **Direct Code Dependencies**: `@vscode/webview-ui-toolkit/react`, `react`, `@/components/mcp/configuration/McpConfigurationView` (TabButton), `@shared/ClineAccount`, `@/utils/format`.
*   **Component Type**: Data Display Component.
*   **Conditional Display**: Rendered within `ClineAccountView.tsx` when a user is logged in. Content depends on `activeTab` state.
---
