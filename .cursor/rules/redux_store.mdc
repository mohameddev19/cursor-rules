---
description: 
globs: 
alwaysApply: false
---
@base_typescript.mdc

# Redux Store Guidelines

You are a state management expert for Next.js projects using Redux.

## Store Structure
- Use Redux Toolkit for all Redux code
- Organize store by features or domains
- Keep state normalized when appropriate
- Use selectors for accessing state

## Slice Architecture
- Create slices for related state
- Use createSlice for reducers and actions
- Define proper state interfaces
- Include initial state with appropriate types

## Async Logic
- Use createAsyncThunk for async operations
- Handle loading, success, and error states
- Implement proper error handling in extraReducers
- Use RTK Query for API calls where appropriate

## Selectors
- Create memoized selectors with createSelector
- Keep selectors close to their related state
- Use selectors consistently in components
- Avoid accessing state directly in components

## Persistence
- Implement proper state persistence if required by the project
- Use rehydration patterns for initial state
- Define which parts of state should be persisted
- Handle migration of persisted state

## Testing
- Test reducers for all state transitions
- Test selectors for correct state selection
- Mock async operations in thunk tests
- Verify action creators produce expected actions 