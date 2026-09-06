# Contributing to Your Library Name

Thank you for your interest in contributing! We welcome bug reports, feature requests, documentation improvements, and pull requests from the community.

Because this library implements the **IEEE 1788.1-2017 Standard for Interval Arithmetic**, maintaining strict standards compliance, mathematical correctness, and test suite integrity is our top priority.

---

## Code of Conduct

Please maintain a welcoming, respectful, and constructive environment in all issues, discussions, and pull requests.

---

## How to Report Bugs or Spec Non-Conformance

If you identify a bug, a calculation error, or a deviation from IEEE 1788.1-2017:

1. **Search existing issues** to see if the problem has already been reported.
2. If not, **open a new issue** with:
   - A clear, descriptive title.
   - The exact interval operations and inputs that produce unexpected behavior.
   - The observed behavior vs. the expected behavior according to the IEEE 1788.1 specification.
   - Your Python version and operating system.

---

## Setting Up Your Development Environment

1. **Fork and clone the repository:**
   ```bash
   git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
   cd your-repo-name
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install the package in editable mode with development dependencies:**
   ```bash
   pip install -e .
   pip install pytest
   ```

---

## Running the Test Suite

We use **pytest** to run our test suite, which includes tests derived from the **ITF1788** test suite.

Before submitting any code changes or Pull Requests, verify that all tests pass locally:

```bash
python -m pytest
```

If you add a new feature or fix a bug:
- Add corresponding unit tests under `tests/`.
- Ensure no existing ITF1788 compliance tests break or are skipped without justification.

---

## Code Style & Standards

To ensure the repository remains clean and maintainable:

- **Standards Conformance:** Any change to interval bounds, set relations, decorations, string parsing, or arithmetic operations **must** strictly conform to IEEE 1788.1-2017 rules.
- **Type Hints:** Use explicit Python type hints on all public function and method signatures.
- **Docstrings:** Use structured docstrings (preferably NumPy style) for public APIs, detailing parameters, return values, and potential exceptions.

---

## Submitting a Pull Request (PR)

1. Create a feature or bugfix branch:
   ```bash
   git checkout -b feature/my-feature-name
   ```
2. Commit your changes with clear, descriptive commit messages.
3. Push to your fork and open a Pull Request against the `main` branch.
4. Verify that all **GitHub Actions CI checks pass** on your PR.
