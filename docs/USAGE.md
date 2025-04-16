# Using Next.js Project Cursor Rules

This document provides detailed instructions on how to implement and use the Next.js project cursor rules in your own projects.

## Table of Contents
- [Installation](#installation)
  - [Basic Setup](#basic-setup)
  - [Validating Installation](#validating-installation)
- [Customizing Rules for Your Project](#customizing-rules-for-your-project)
  - [UI Component Library](#ui-component-library)
  - [State Management](#state-management)
  - [Internationalization](#internationalization)
  - [Project-Specific Standards](#project-specific-standards)
- [Working with Rules in Your Project](#working-with-rules-in-your-project)
  - [Consistent Component Creation](#consistent-component-creation)
  - [Page Creation](#page-creation)
  - [Form Implementation](#form-implementation)
  - [Business Logic Organization](#business-logic-organization)
- [Practical Examples with Claude](#practical-examples-with-claude)
- [Advanced Usage](#advanced-usage)
  - [Rule Files Reference](#rule-files-reference)
  - [Glob Patterns](#glob-patterns)
  - [Always Apply](#always-apply)
- [Troubleshooting](#troubleshooting)
- [Contributing Back](#contributing-back)

## Installation

### Basic Setup

1. Copy the `.cursorrules` file to the root of your Next.js project:
   ```bash
   cp .cursorrules /path/to/your/project/
   ```

2. Create the `.cursor/rules` directory in your project:
   ```bash
   mkdir -p /path/to/your/project/.cursor/rules
   ```

3. Copy all rule files to your project:
   ```bash
   cp -r .cursor/rules/* /path/to/your/project/.cursor/rules/
   ```

### Validating Installation

To confirm the rules are properly installed:

1. Open your project in Cursor IDE
2. Open any relevant file (e.g., a React component)
3. Ask Claude about your project standards (e.g., "What are the component guidelines for this project?")
4. Claude should respond with information from the relevant rule files

## Customizing Rules for Your Project

The rule files are designed to be customizable. Here's how to adapt them to your project:

### UI Component Library

If your project uses a specific UI library (e.g., Mantine, shadcn/ui, Material UI):

1. Open `.cursorrules` and review the "Framework Standards" section
2. Specify your UI library in the "Utilize UI component library" bullet point
3. Make sure any component-specific rules match your chosen library

### State Management

If your project uses a specific state management approach:

1. Open `.cursorrules` and update the "Use state management approach" bullet point
2. If using Redux, keep the `redux_store.mdc` file
3. If using a different approach (Context API, Zustand, etc.), you may want to create a new rule file specific to that approach

### Internationalization

If your project requires multi-language support:

1. Ensure the i18n bullet points reflect your project's language requirements
2. Update any RTL-specific guidance if your project supports RTL languages

### Project-Specific Standards

You may want to add project-specific standards:

1. Create new `.mdc` files in the `.cursor/rules` directory for project-specific conventions
2. Follow the existing format with frontmatter and markdown sections
3. Reference other rule files using the `@filename.mdc` syntax if appropriate

## Working with Rules in Your Project

### Consistent Component Creation

When creating new components:

1. Reference the React component guidelines in `.cursor/rules/react_components.mdc`
2. Follow the component structure pattern
3. Use state management as specified in your rules
4. Implement styling according to your UI library conventions

### Page Creation

When creating new pages:

1. Follow the Next.js page guidelines in `.cursor/rules/nextjs_pages.mdc`
2. Structure your pages according to the App Router conventions
3. Implement proper data fetching strategies
4. Handle routing, SEO, and error states as specified

### Form Implementation

When implementing forms:

1. Reference the form component guidelines in `.cursor/rules/components/form_components.mdc`
2. Use the form management library specified for your project
3. Follow validation and accessibility guidelines
4. Structure your form components consistently

### Business Logic Organization

When adding business logic:

1. Follow the service layer guidelines in `.cursor/rules/services.mdc`
2. Separate concerns between UI, state, and business logic
3. Implement proper TypeScript types for service inputs and outputs
4. Follow asynchronous operation patterns as specified

## Practical Examples with Claude

Here are some concrete examples of how to leverage these rules with Claude in Cursor:

### Example 1: Creating a Component

**Your prompt to Claude:**
```
Create a product card component that shows an image, title, price, and add to cart button.
```

**Claude's response with rules:**
Claude will create a component that:
- Uses your project's UI library components
- Follows your TypeScript conventions
- Has proper prop typing
- Includes accessibility attributes
- Uses the 'use client' directive if appropriate
- Follows your project's styling approach

### Example 2: Adding Form Validation

**Your prompt to Claude:**
```
Add validation to my user registration form to validate email, password strength, and matching password confirmation.
```

**Claude's response with rules:**
Claude will:
- Use your project's form validation library (Zod, Yup, etc.)
- Follow your error handling patterns
- Implement appropriate client-side validation
- Include proper error messaging
- Follow your form styling conventions

### Example 3: Creating a New Page

**Your prompt to Claude:**
```
Create a product details page that fetches data for a specific product ID.
```

**Claude's response with rules:**
Claude will create a page that:
- Uses App Router conventions
- Implements proper data fetching with caching
- Sets up correct metadata for SEO
- Handles loading and error states
- Follows your project's page layout structure

## Advanced Usage

### Rule Files Reference

You can use the `@filename.mdc` syntax to reference other rule files within your rule files. This is useful for building complex rule sets that share common principles.

Example:
```
---
description: My Custom Rules
globs:
alwaysApply: false
---
@base_typescript.mdc

# My Custom Rules
...
```

### Glob Patterns

You can use glob patterns in the frontmatter to specify which files a rule applies to:

```
---
description: Component Rules
globs:
- 'components/**/*.tsx'
- 'app/**/components/**/*.tsx'
alwaysApply: false
---
```

### Always Apply

If you want a rule to always be applied regardless of which file is currently open, set `alwaysApply: true` in the frontmatter.

## Troubleshooting

If Claude doesn't seem to be following your rules:

1. Verify the rule files are in the correct location (`.cursor/rules/`)
2. Check that the file extension is `.mdc`
3. Validate that the frontmatter is correctly formatted
4. Try explicitly mentioning the rule in your prompt to Claude

## Contributing Back

If you make improvements to these rules that might benefit others:

1. Consider submitting a pull request to the original repository
2. Document your changes and why they're beneficial
3. Make sure any project-specific details are removed 