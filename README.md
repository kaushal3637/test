## Stylus Analyzer

Details

We are building StylusAnalyzer, a security tool for auditing Rust-based smart contracts written using Stylus on Arbitrum. The goal is to help developers catch bugs and security risks before deployment, making it easier to launch safe, reliable applications.
It performs static analysis to identify common vulnerabilities, including:
Access control and authorization issues
Reentrancy and unsafe external calls
Arithmetic errors and state inconsistencies
Problems in asset handling and transfers
Violations of coding standards and best practices
Unsafe transaction flow and control logic
Storage layout and memory safety concerns
In addition to static checks, StylusAnalyzer offers AI-powered auditing assistance. The tool not only detects where problems occur in the code but also suggests safe, actionable fixes. It also streamlines the audit process by automatically generating clear, professional PDF reports, helping both developers and auditors save valuable time.

To support this work, we have already built a working MVP that includes several early vulnerability detectors, a basic AI module that provides suggestions on identified issues, and a functioning PDF report generator. The MVP is open-sourced and available on GitHub(
) and PyPI(
).
What innovation or value will your project bring to Arbitrum? What previously unaddressed problems is it solving? Is the project introducing genuinely new mechanisms

Smart contract security is a critical need for any blockchain ecosystem, but while Stylus opens powerful new possibilities for Rust developers on Arbitrum, the security tooling available today remains limited. Most existing audit tools are focused on Solidity and are either incompatible or ineffective when used with Rust-based contracts. This creates a clear gap for developers working with Stylus.

StylusAnalyzer is built to close this gap. It’s a dedicated tool for Rust-written smart contracts that combines static vulnerability detection, AI-assisted auditing, and automated report generation, all tailored to the Stylus environment. It’s not a repurposed Solidity scanner; it’s designed from the ground up with Stylus-specific risk patterns in mind.

StylusAnalyzer introduces meaningful new mechanisms to Arbitrum by:

Offering precise static analysis designed for Stylus contract structures
Using AI not just to detect, but to guide and explain issues clearly
Generating instant, professional-quality audit reports
Reducing the time and effort developers spend on manual review
By improving contract safety, speeding up audits, and supporting teams without access to expensive audit services, StylusAnalyzer helps make Arbitrum a more secure and developer-friendly place to build, especially for the growing Rust developer community.

What is the current stage of your project

We're currently at the MVP stage for StylusAnalyzer. This includes an initial set of static vulnerability detectors, AI-assisted auditing, and automated PDF report generation. While the AI module currently offers basic guidance on detected issues, it's still early-stage and will be further refined with better training and contextual understanding as we progress towards production readiness. The report generation feature works, but we plan to enhance its formatting and depth for professional-grade outputs.

For static analysis, we’ve built several detectors by converting Rust-based Stylus contracts into ASTs and analyzing patterns commonly associated with vulnerabilities. These include:

UncheckedTransferDetector: Identifies token transfers (transfer or transfer_from) where the return value is not checked, which can result in silent failures and inconsistent contract state.
Unsafe use of .unwrap(): Detects unsafe usage of .unwrap() on Option or Result types, which can cause panics if the value is None or Err. This helps prevent avoidable transaction failures and DoS vectors.
PanicDetector: Flags direct uses of the panic!() macro. In smart contracts, panics are unrecoverable and result in full transaction reverts; instead, developers should favor graceful error handling with Result or Option.
Reentrancy Feature Check: Stylus contracts support reentrancy protection by default, but if the features = ["reentrant"] flag is set, our tool warns the developer about possible reentrancy risks in the absence of proper guards.
EncodePackedDetector: Finds unsafe uses of encode_packed with dynamic types like strings, which can lead to hash collisions (e.g., encode_packed("a", "bc") == encode_packed("ab", "c")). This is especially dangerous in logic involving signatures or unique identifiers.
LockedEtherDetector: Flags contracts that can receive Ether but lack withdrawal methods, potentially causing funds to become permanently inaccessible.