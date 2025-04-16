# Next.js Project Cursor Rules: Practical Examples

This document provides real-world examples of how using cursor rules can enhance Claude's assistance for your Next.js projects.

## Table of Contents
- [Component Creation Examples](#component-creation-examples)
- [Page Development Examples](#page-development-examples)
- [Form Implementation Examples](#form-implementation-examples)
- [State Management Examples](#state-management-examples)
- [Service Layer Examples](#service-layer-examples)
- [Best Practices Advice](#best-practices-advice)

## Component Creation Examples

### Example 1: Basic UI Component

**Your prompt to Claude:**
```
Create a ProfileCard component showing a user's avatar, name, email, and role with a follow button.
```

**Without rules:**
Claude might create a basic React component without any specific UI library or project conventions.

```tsx
// A generic component with no specific conventions or libraries
const ProfileCard = ({ name, email, avatar, role }) => {
  return (
    <div className="profile-card">
      <img src={avatar} alt={`${name}'s avatar`} />
      <h3>{name}</h3>
      <p>{email}</p>
      <span>{role}</span>
      <button onClick={() => console.log('Followed')}>Follow</button>
    </div>
  );
};

export default ProfileCard;
```

**With rules:**
Claude will follow your project's UI library conventions (e.g., Mantine), use TypeScript types, and follow your component structure pattern.

```tsx
'use client';

import { Avatar, Badge, Button, Card, Group, Stack, Text } from '@mantine/core';
import { useState } from 'react';

interface IProfileCardProps {
  name: string;
  email: string;
  avatar: string;
  role: string;
  onFollow?: (followed: boolean) => void;
}

export const ProfileCard = ({ 
  name, 
  email, 
  avatar, 
  role,
  onFollow 
}: IProfileCardProps) => {
  const [followed, setFollowed] = useState(false);
  
  const handleFollow = () => {
    const newFollowState = !followed;
    setFollowed(newFollowState);
    onFollow?.(newFollowState);
  };
  
  return (
    <Card withBorder padding="lg" radius="md">
      <Group>
        <Avatar src={avatar} size="xl" radius="xl" alt={`${name}'s avatar`} />
        <Stack gap="xs">
          <Text fw={500} size="lg">{name}</Text>
          <Text size="sm" c="dimmed">{email}</Text>
          <Badge>{role}</Badge>
        </Stack>
      </Group>
      <Button 
        variant={followed ? "filled" : "outline"} 
        onClick={handleFollow} 
        fullWidth 
        mt="md"
      >
        {followed ? 'Following' : 'Follow'}
      </Button>
    </Card>
  );
};
```

### Example 2: Data Display Component

**Your prompt to Claude:**
```
Create a DataTable component for showing user activity logs with timestamp, action, and status columns.
```

**Without rules:**
Claude might create a simple HTML table without specific conventions.

**With rules:**
Claude will create a component that follows your project's patterns, including proper TypeScript typing and accessibility standards.

```tsx
'use client';

import { Table, Badge, Text, Group, ScrollArea } from '@mantine/core';
import { format } from 'date-fns';

export interface IActivityLog {
  id: string;
  timestamp: Date;
  action: string;
  status: 'success' | 'pending' | 'failed';
  userId: string;
  details?: string;
}

interface IActivityLogTableProps {
  logs: IActivityLog[];
  loading?: boolean;
}

export const ActivityLogTable = ({ logs, loading = false }: IActivityLogTableProps) => {
  const getStatusColor = (status: IActivityLog['status']) => {
    switch (status) {
      case 'success': return 'green';
      case 'pending': return 'yellow';
      case 'failed': return 'red';
      default: return 'gray';
    }
  };

  return (
    <ScrollArea>
      <Table striped highlightOnHover>
        <Table.Thead>
          <Table.Tr>
            <Table.Th>Timestamp</Table.Th>
            <Table.Th>Action</Table.Th>
            <Table.Th>Status</Table.Th>
            <Table.Th>Details</Table.Th>
          </Table.Tr>
        </Table.Thead>
        <Table.Tbody>
          {loading ? (
            <Table.Tr>
              <Table.Td colSpan={4}>
                <Text ta="center">Loading activity logs...</Text>
              </Table.Td>
            </Table.Tr>
          ) : logs.length === 0 ? (
            <Table.Tr>
              <Table.Td colSpan={4}>
                <Text ta="center">No activity logs found</Text>
              </Table.Td>
            </Table.Tr>
          ) : (
            logs.map((log) => (
              <Table.Tr key={log.id}>
                <Table.Td>{format(log.timestamp, 'MMM d, yyyy HH:mm:ss')}</Table.Td>
                <Table.Td>{log.action}</Table.Td>
                <Table.Td>
                  <Badge color={getStatusColor(log.status)}>
                    {log.status}
                  </Badge>
                </Table.Td>
                <Table.Td>{log.details || '-'}</Table.Td>
              </Table.Tr>
            ))
          )}
        </Table.Tbody>
      </Table>
    </ScrollArea>
  );
};
```

## Page Development Examples

### Example: Product Detail Page

**Your prompt to Claude:**
```
Create a product detail page that fetches product information based on the URL parameter.
```

**Without rules:**
Claude might create a basic page without following Next.js App Router conventions or proper data fetching.

**With rules:**
Claude will create a properly structured page with appropriate data fetching, loading states, and error handling.

```tsx
// app/products/[id]/page.tsx
import { Suspense } from 'react';
import { notFound } from 'next/navigation';
import { Metadata } from 'next';
import ProductDetails from '@/components/products/ProductDetails';
import ProductRecommendations from '@/components/products/ProductRecommendations';
import { getProductById } from '@/services/productService';
import LoadingProductSkeleton from '@/components/ui/LoadingProductSkeleton';
import ErrorBoundary from '@/components/ui/ErrorBoundary';

interface ProductPageProps {
  params: {
    id: string;
  };
}

export async function generateMetadata(
  { params }: ProductPageProps
): Promise<Metadata> {
  try {
    const product = await getProductById(params.id);
    
    if (!product) {
      return {
        title: 'Product Not Found',
      };
    }
    
    return {
      title: `${product.name} | Our Store`,
      description: product.description.substring(0, 160),
      openGraph: {
        images: [{ url: product.images[0].url }],
      },
    };
  } catch (error) {
    return {
      title: 'Product Details',
    };
  }
}

export default async function ProductPage({ params }: ProductPageProps) {
  const product = await getProductById(params.id);
  
  if (!product) {
    notFound();
  }
  
  return (
    <main className="container mx-auto py-8 px-4">
      <ErrorBoundary fallback={<div>Something went wrong loading this product.</div>}>
        <Suspense fallback={<LoadingProductSkeleton />}>
          <ProductDetails product={product} />
        </Suspense>
        
        <div className="mt-16">
          <h2 className="text-2xl font-bold mb-4">You might also like</h2>
          <Suspense fallback={<p>Loading recommendations...</p>}>
            <ProductRecommendations productId={params.id} />
          </Suspense>
        </div>
      </ErrorBoundary>
    </main>
  );
}
```

## Form Implementation Examples

### Example: User Registration Form

**Your prompt to Claude:**
```
Create a registration form with fields for name, email, password, confirm password, and a terms acceptance checkbox.
```

**Without rules:**
Claude might create a basic form without specific libraries or validation.

**With rules:**
Claude will create a form using your project's form library, with proper validation and error handling.

```tsx
'use client';

import { useState } from 'react';
import { useForm, zodResolver } from '@mantine/form';
import { z } from 'zod';
import { 
  TextInput, 
  PasswordInput, 
  Checkbox, 
  Button, 
  Box, 
  Group, 
  Stack,
  Title,
  Text,
  Alert
} from '@mantine/core';
import { IconAlertCircle } from '@tabler/icons-react';
import { registerUser } from '@/services/authService';

// Form validation schema
const registerSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email address'),
  password: z
    .string()
    .min(8, 'Password must be at least 8 characters')
    .regex(/[A-Z]/, 'Password must contain at least one uppercase letter')
    .regex(/[a-z]/, 'Password must contain at least one lowercase letter')
    .regex(/[0-9]/, 'Password must contain at least one number'),
  confirmPassword: z.string(),
  acceptTerms: z.boolean().refine(val => val === true, {
    message: 'You must accept the terms and conditions'
  })
}).refine(data => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ['confirmPassword']
});

type RegisterFormValues = z.infer<typeof registerSchema>;

export function RegisterForm() {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  const form = useForm<RegisterFormValues>({
    initialValues: {
      name: '',
      email: '',
      password: '',
      confirmPassword: '',
      acceptTerms: false
    },
    validate: zodResolver(registerSchema)
  });
  
  const handleSubmit = async (values: RegisterFormValues) => {
    setIsSubmitting(true);
    setError(null);
    
    try {
      await registerUser({
        name: values.name,
        email: values.email,
        password: values.password
      });
      
      // Redirect or show success message
      window.location.href = '/login?registered=true';
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Registration failed. Please try again.');
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <Box mx="auto" sx={{ maxWidth: 400 }} p="md">
      <Title order={2} mb="md">Create an account</Title>
      
      {error && (
        <Alert icon={<IconAlertCircle size={16} />} color="red" mb="md">
          {error}
        </Alert>
      )}
      
      <form onSubmit={form.onSubmit(handleSubmit)}>
        <Stack spacing="md">
          <TextInput
            required
            label="Full Name"
            placeholder="John Doe"
            {...form.getInputProps('name')}
          />
          
          <TextInput
            required
            label="Email"
            placeholder="your@email.com"
            {...form.getInputProps('email')}
          />
          
          <PasswordInput
            required
            label="Password"
            placeholder="Your password"
            {...form.getInputProps('password')}
          />
          
          <PasswordInput
            required
            label="Confirm Password"
            placeholder="Confirm your password"
            {...form.getInputProps('confirmPassword')}
          />
          
          <Checkbox
            label="I agree to the terms and conditions"
            {...form.getInputProps('acceptTerms', { type: 'checkbox' })}
          />
          
          <Group position="apart" mt="lg">
            <Text component="a" href="/login" size="sm">
              Already have an account?
            </Text>
            <Button type="submit" loading={isSubmitting}>
              Register
            </Button>
          </Group>
        </Stack>
      </form>
    </Box>
  );
}
```

## State Management Examples

### Example: Redux Store for Products

**Your prompt to Claude:**
```
Create a Redux slice for managing products with loading, adding, updating, and removing functionality.
```

**Without rules:**
Claude might create a basic Redux setup without following best practices.

**With rules:**
Claude will create a properly structured Redux slice following your project's state management patterns.

```tsx
// src/redux/slices/productsSlice.ts
import { createSlice, createAsyncThunk, PayloadAction } from '@reduxjs/toolkit';
import { 
  fetchProductsApi, 
  addProductApi, 
  updateProductApi, 
  deleteProductApi 
} from '@/services/api/productApi';

export interface IProduct {
  id: string;
  name: string;
  description: string;
  price: number;
  category: string;
  imageUrl: string;
  inStock: boolean;
}

interface ProductsState {
  items: IProduct[];
  selectedProduct: IProduct | null;
  status: 'idle' | 'loading' | 'succeeded' | 'failed';
  error: string | null;
}

const initialState: ProductsState = {
  items: [],
  selectedProduct: null,
  status: 'idle',
  error: null
};

// Async thunks
export const fetchProducts = createAsyncThunk(
  'products/fetchProducts',
  async (_, { rejectWithValue }) => {
    try {
      return await fetchProductsApi();
    } catch (error) {
      return rejectWithValue(error instanceof Error ? error.message : 'Failed to fetch products');
    }
  }
);

export const addProduct = createAsyncThunk(
  'products/addProduct',
  async (product: Omit<IProduct, 'id'>, { rejectWithValue }) => {
    try {
      return await addProductApi(product);
    } catch (error) {
      return rejectWithValue(error instanceof Error ? error.message : 'Failed to add product');
    }
  }
);

export const updateProduct = createAsyncThunk(
  'products/updateProduct',
  async (product: IProduct, { rejectWithValue }) => {
    try {
      return await updateProductApi(product);
    } catch (error) {
      return rejectWithValue(error instanceof Error ? error.message : 'Failed to update product');
    }
  }
);

export const deleteProduct = createAsyncThunk(
  'products/deleteProduct',
  async (id: string, { rejectWithValue }) => {
    try {
      await deleteProductApi(id);
      return id;
    } catch (error) {
      return rejectWithValue(error instanceof Error ? error.message : 'Failed to delete product');
    }
  }
);

const productsSlice = createSlice({
  name: 'products',
  initialState,
  reducers: {
    selectProduct: (state, action: PayloadAction<string>) => {
      state.selectedProduct = state.items.find(product => product.id === action.payload) || null;
    },
    clearSelectedProduct: (state) => {
      state.selectedProduct = null;
    },
    setProductFilter: (state, action) => {
      // Filter functionality could be added here
    }
  },
  extraReducers: (builder) => {
    builder
      // Fetch products
      .addCase(fetchProducts.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.items = action.payload;
        state.error = null;
      })
      .addCase(fetchProducts.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.payload as string;
      })
      
      // Add product
      .addCase(addProduct.fulfilled, (state, action) => {
        state.items.push(action.payload);
        state.error = null;
      })
      .addCase(addProduct.rejected, (state, action) => {
        state.error = action.payload as string;
      })
      
      // Update product
      .addCase(updateProduct.fulfilled, (state, action) => {
        const index = state.items.findIndex(product => product.id === action.payload.id);
        if (index !== -1) {
          state.items[index] = action.payload;
        }
        if (state.selectedProduct?.id === action.payload.id) {
          state.selectedProduct = action.payload;
        }
        state.error = null;
      })
      .addCase(updateProduct.rejected, (state, action) => {
        state.error = action.payload as string;
      })
      
      // Delete product
      .addCase(deleteProduct.fulfilled, (state, action) => {
        state.items = state.items.filter(product => product.id !== action.payload);
        if (state.selectedProduct?.id === action.payload) {
          state.selectedProduct = null;
        }
        state.error = null;
      })
      .addCase(deleteProduct.rejected, (state, action) => {
        state.error = action.payload as string;
      });
  }
});

export const { selectProduct, clearSelectedProduct, setProductFilter } = productsSlice.actions;

// Selectors
export const selectAllProducts = (state) => state.products.items;
export const selectProductById = (state, productId) => 
  state.products.items.find(product => product.id === productId);
export const selectProductsStatus = (state) => state.products.status;
export const selectProductsError = (state) => state.products.error;
export const selectSelectedProduct = (state) => state.products.selectedProduct;

export default productsSlice.reducer;
```

## Service Layer Examples

### Example: Authentication Service

**Your prompt to Claude:**
```
Create an authentication service with login, registration, password reset, and session management.
```

**Without rules:**
Claude might create a simple auth service without proper patterns.

**With rules:**
Claude will create a well-structured service following your project's service layer guidelines.

```tsx
// services/authService.ts
import { cookies } from 'next/headers';

export interface IUser {
  id: string;
  name: string;
  email: string;
  role: 'user' | 'admin';
}

export interface ILoginCredentials {
  email: string;
  password: string;
}

export interface IRegistrationData {
  name: string;
  email: string;
  password: string;
}

export interface IAuthResult {
  user: IUser;
  token: string;
}

export interface IResetPasswordRequest {
  email: string;
}

export interface IChangePasswordRequest {
  token: string;
  newPassword: string;
}

export class AuthService {
  private apiUrl: string;
  
  constructor() {
    this.apiUrl = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000/api';
  }
  
  /**
   * Log in a user with email and password
   */
  async login(credentials: ILoginCredentials): Promise<IAuthResult> {
    try {
      const response = await fetch(`${this.apiUrl}/auth/login`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(credentials),
      });
      
      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || 'Login failed');
      }
      
      return await response.json();
    } catch (error) {
      console.error('Login error:', error);
      throw error;
    }
  }
  
  /**
   * Register a new user
   */
  async registerUser(data: IRegistrationData): Promise<IAuthResult> {
    try {
      const response = await fetch(`${this.apiUrl}/auth/register`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(data),
      });
      
      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || 'Registration failed');
      }
      
      return await response.json();
    } catch (error) {
      console.error('Registration error:', error);
      throw error;
    }
  }
  
  /**
   * Request password reset email
   */
  async requestPasswordReset(data: IResetPasswordRequest): Promise<{ success: boolean; message: string }> {
    try {
      const response = await fetch(`${this.apiUrl}/auth/reset-password`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(data),
      });
      
      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || 'Password reset request failed');
      }
      
      return await response.json();
    } catch (error) {
      console.error('Password reset request error:', error);
      throw error;
    }
  }
  
  /**
   * Reset password with token
   */
  async resetPassword(data: IChangePasswordRequest): Promise<{ success: boolean; message: string }> {
    try {
      const response = await fetch(`${this.apiUrl}/auth/change-password`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(data),
      });
      
      if (!response.ok) {
        const error = await response.json();
        throw new Error(error.message || 'Password change failed');
      }
      
      return await response.json();
    } catch (error) {
      console.error('Password change error:', error);
      throw error;
    }
  }
  
  /**
   * Get the current authenticated user
   */
  async getCurrentUser(): Promise<IUser | null> {
    try {
      const token = this.getToken();
      
      if (!token) {
        return null;
      }
      
      const response = await fetch(`${this.apiUrl}/auth/me`, {
        headers: {
          'Authorization': `Bearer ${token}`,
        },
      });
      
      if (!response.ok) {
        throw new Error('Failed to get current user');
      }
      
      return await response.json();
    } catch (error) {
      console.error('Get current user error:', error);
      return null;
    }
  }
  
  /**
   * Log out the current user
   */
  async logout(): Promise<void> {
    // Client-side cookie removal
    if (typeof window !== 'undefined') {
      document.cookie = 'auth_token=; Max-Age=0; path=/; domain=' + window.location.hostname;
    }
    
    try {
      // Also notify the backend
      const token = this.getToken();
      
      if (token) {
        await fetch(`${this.apiUrl}/auth/logout`, {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${token}`,
          },
        });
      }
    } catch (error) {
      console.error('Logout error:', error);
    }
  }
  
  /**
   * Store auth token in cookie
   */
  private storeToken(token: string): void {
    // Set cookie that expires in 7 days
    const expiresIn = 60 * 60 * 24 * 7; // 7 days in seconds
    document.cookie = `auth_token=${token}; Max-Age=${expiresIn}; path=/; secure; samesite=strict`;
  }
  
  /**
   * Get auth token from cookie
   */
  private getToken(): string | null {
    if (typeof window === 'undefined') {
      // Server-side
      return null;
    }
    
    const cookies = document.cookie.split(';');
    for (let cookie of cookies) {
      const [name, value] = cookie.trim().split('=');
      if (name === 'auth_token') {
        return value;
      }
    }
    return null;
  }
}

// Singleton instance
const authService = new AuthService();
export default authService;
```

## Best Practices Advice

### Example: Performance Optimization

**Your prompt to Claude:**
```
What optimizations can I make to improve the performance of my Next.js application?
```

**Without rules:**
Claude might provide generic performance advice.

**With rules:**
Claude will provide specific, framework-aligned advice that follows your project's guidelines.

```
Based on your project's guidelines, here are targeted optimizations for your Next.js application:

1. Image Optimization
   - Replace all <img> tags with Next.js Image component
   - Set proper sizes attributes for responsive images
   - Use quality settings appropriate for your image content

2. Rendering Strategy Improvements
   - Convert UI-only components to Client Components with 'use client'
   - Keep data-fetching logic in Server Components where possible
   - Use proper suspense boundaries with <Suspense> for loading states

3. Data Fetching Optimizations
   - Implement proper fetch cache policies in Server Components:
     ```ts
     // Revalidate every hour
     fetch(url, { next: { revalidate: 3600 } })
     
     // Force-cache (default)
     fetch(url, { cache: 'force-cache' })
     
     // No cache
     fetch(url, { cache: 'no-store' })
     ```
   - Use React Query/SWR for client-side data fetching with proper caching

4. Component Optimization
   - Apply React.memo for pure components that re-render frequently
   - Use useMemo for expensive calculations
   - Implement useCallback for event handlers
   - Fix unnecessary re-renders by optimizing dependency arrays

5. Code Splitting
   - Use dynamic imports for large components not needed immediately
   - Implement route-based code splitting
   - Lazy load below-the-fold content

6. State Management
   - Keep Redux state normalized and flat
   - Use selectors with createSelector for memoization
   - Avoid storing derived data in state
```

This document demonstrates how Claude's assistance is enhanced when it understands your project's specific requirements and conventions through cursor rules. 