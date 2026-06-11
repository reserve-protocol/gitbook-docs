# Risks

This section explains the risks of using the Reserve platform. If you interact with the platform, you do so at your own risk. The following is provided for informational purposes only and is intended to describe certain risks associated with accessing, using, or otherwise interacting with the Reserve platform, including its smart contracts, related interfaces, and any DTFs. Nothing here or anywhere else on reserve.org or app.reserve.org (the “App”) is a recommendation to purchase, hold, redeem, mint, stake, trade, or otherwise use any DTF, protocol, or service. We believe clearly describing the following risks helps users make informed decisions. If you are unsure about any risk, you can ask questions in [Telegram](https://t.me/reservecurrency).

These risks are not, and should not be construed as, exhaustive. They do not identify all risks, whether known or unknown, existing now or arising in the future. Users should conduct their own independent review and diligence before interacting with the Reserve platform and any DTF and should remain informed of any changes to the protocol, its dependencies, governance arrangements, or related infrastructure. If you do not fully understand the applicable risks, you should not use the Reserve platform.

## Reserve platform risks: smart contracts

The Reserve platform relies on smart contracts to operate. Smart contracts may contain coding errors, design defects, security vulnerabilities, or other faults, whether known or unknown, that may result in exploits, unauthorized transactions, malfunction, interruption, inaccurate operation, or total or partial loss of user assets. Although smart contracts associated with the Reserve platform may be reviewed, tested, or audited by third parties, no audit, review, or testing process can eliminate all vulnerabilities or guarantee that the smart contracts are secure, functional, or free from defects.

## Oracle risks

DTFs rely on third-party and other external pricing inputs, including oracle services, to obtain real-time or near-real-time asset price information. If any oracle fails, becomes unavailable, is manipulated, publishes inaccurate, stale, incomplete, or delayed data, or is integrated or used improperly, the Reserve platform may operate incorrectly. Such failures may affect accounting, collateral management, default determinations, recollateralization actions, redemptions, trading behavior, or other core protocol functions. By way of example only, an inaccurate price report for USDC could cause a Yield DTF to incorrectly treat a collateral asset as defaulted and attempt to exchange that asset for emergency collateral, potentially at a loss.

## MEV, slippage, and transaction execution risks

Transactions submitted to public blockchains may be subject to miner or maximal extractable value practices, including sandwich attacks, front-running, back-running, and other forms of transaction reordering or opportunistic extraction. Users interacting with automated market makers or similar trading venues in connection with the Reserve platform, including for DTF trading, may incur losses as a result of adverse price movement, slippage, failed execution, partial execution, or value extraction by third parties. Users are solely responsible for evaluating transaction settings, including slippage tolerances, and for determining whether to use any available transaction protection tools or alternative transaction-routing methods.

## Governance risks

The Reserve team has deployed a suggested governance system for DTFs that enables fully onchain governance. Because governance powers are broad, governance attacks are possible. For example, an attacker who acquires enough voting power could approve a malicious upgrade that enables theft of funds. Some of this risk is mitigated by special roles within the governance system. That said, these protections only work if the people holding those roles exercise them competently and in good faith. Before using a DTF, users should review who holds these roles on the Details + Roles page and the Governance page in the App.

## Issuer and custodian risk

Digital assets may depend on issuers, custodians, or other centralized or third-party actors. Such parties may impose transfer restrictions, freezes, blacklists, redemption limits, or other controls that could impair the collateral assets underlying DTFs. These powers may be exercised for regulatory, compliance, commercial, discretionary, or other reasons, including reasons unrelated to any particular user conduct. In addition, the value and stability of certain collateral assets may depend on the adequacy, existence, management, and accessibility of offchain reserves or other supporting assets. Those arrangements may fail, be mismanaged, be misrepresented, become illiquid, or otherwise prove insufficient.

## Price and market risk

The value of a DTF generally reflects the weighted value of its underlying collateral assets. If one or more underlying assets declines in value, depegs, becomes impaired, or experiences volatility, dislocation, or market stress, the value of the DTF may decline correspondingly. DTFs backed by volatile or partially volatile assets may experience substantial price fluctuations, and users should not assume price stability unless expressly stated and continuously maintained, which cannot be guaranteed. In addition, where Yield DTFs rely on RSR or similar overcollateralization mechanisms, such protection may be insufficient in practice. In stressed market conditions, the market value, liquidity, or realizable sale proceeds of RSR may decline materially, which may reduce or eliminate the effectiveness of any overcollateralization and impair the protocol’s ability to recollateralize after a default or collateral event.

## Underlying protocol risk

The Reserve platform and DTFs may rely on assets, services, or integrations from third-party protocols, applications, bridges, liquidity venues, or infrastructure providers, including lending protocols and other decentralized finance systems. Use of the Reserve platform therefore exposes users to the risks associated with those third-party systems, including smart contract failure, governance failure, insolvency, exploit, oracle error, liquidity failure, bridge failure, upgrade risk, dependency failure, or other operational or legal issues. Failures in any such underlying protocol or integration may directly or indirectly cause loss, delay, impairment, or inaccessibility of collateral assets of DTFs or DTFs themselves.

## Interface and frontend risks

Users may access the Reserve platform through direct smart contract interaction or through one or more web interfaces, applications, or other frontends operated by third parties. Any such interface may contain bugs, security vulnerabilities, malicious code, inaccurate transaction prompts, display errors, integration failures, or other defects. A frontend may also be compromised, spoofed, censored, unavailable, or operated in a malicious or negligent manner. As a result, users may be induced to sign incorrect transactions, approve unintended permissions, send assets to unintended destinations, or otherwise suffer losses. The existence of a publicly available interface does not constitute any representation that the interface is secure, audited, error-free, or appropriate for any particular use.

## No representation; assumption of risk

By using the Reserve platform, you acknowledge that blockchain-based systems are inherently experimental and involve technological, legal, regulatory, and economic uncertainties. You further acknowledge that neither the identification of certain risks nor the implementation of any safeguards, audits, monitoring, governance mechanisms, or emergency procedures eliminates the possibility of loss. You assume all risks associated with use of the Reserve platform and DTFs, including risks arising from smart contracts, governance, oracle dependencies, collateral assets, third-party protocols, user interfaces, market conditions, and user error.
