# Next.js Project Cursor Rules

A collection of standardized guidelines for Next.js projects in Cursor IDE to ensure consistent code quality and best practices.

## Overview

This repository contains Cursor IDE rules for Next.js applications. These rules help maintain consistent code quality and adhere to best practices when developing Next.js applications.

## What are Cursor Rules?

Cursor Rules are guidelines that help AI assistants (like Claude in Cursor) understand your project's conventions and best practices. When you add these rules to your project, Cursor's AI can provide more accurate and useful code suggestions, completions, and refactorings that align with your project's standards.

### Example of Rules in Action

Without rules:
```
User: "Create a component for a form"
Claude: *generates a basic React component with no specific form library or project conventions*
```

With rules:
```
User: "Create a component for a form"
Claude: *generates a component using your project's form library (React Hook Form, Formik, etc.), follows project naming conventions, and includes proper TypeScript types and validation patterns*
```

## How to Use

1. Copy the `.cursorrules` file to the root of your Next.js project
2. Copy the `.cursor/rules` directory to your project
3. Make any project-specific adjustments to the rules as needed

When working with Cursor IDE, the AI will now understand your project's guidelines and provide suggestions that align with these standards.

For detailed installation and customization instructions, see the [Usage Guide](./docs/USAGE.md).

For practical examples of how rules improve Claude's assistance, see the [Examples Document](./docs/EXAMPLES.md).

## Features

These rules provide guidance for:

### Framework Standards
- Next.js 14+ App Router best practices
- Client/server component organization
- TypeScript usage
- UI component library integration
- State management
- Internationalization
- Custom hooks

### Quality Controls
- TypeScript type safety
- ESLint conformance
- Responsive design patterns
- Testing standards
- Code documentation
- Component size management

### Code Organization
- Component responsibility segregation
- Feature-based organization
- Atomic design principles
- Business logic separation
- State management practices

### Performance Guidelines
- Image optimization
- Data fetching strategies
- Rendering optimization
- Memoization patterns
- Code splitting techniques

## Rule Categories

The rules are organized into the following categories:

- **Base TypeScript**: Fundamental TypeScript standards for Next.js projects
- **React Components**: Guidelines for creating React components
- **Form Components**: Best practices for form implementation
- **Next.js Pages**: Standards for page components and routing
- **Redux Store**: Patterns for state management with Redux
- **Services**: Business logic and API integration guidelines

## Customization

These rules are designed to be adaptable to different project requirements. You can modify them to fit your specific project needs:

- Adjust UI library references (Mantine, shadcn/ui, Material UI, etc.)
- Modify state management approaches (Redux, Context API, Zustand, etc.)
- Customize internationalization requirements
- Add project-specific conventions

## License

MIT 