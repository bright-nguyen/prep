# What are props?

> Props (properties) are the pieces of data that are passed from a parent component to its child components and are used to define the data and behaviour of the components.

## Example
```tsx
type HomeProps = {
  name: string;
};
const Home = ({ name }: HomeProps): string => `Hello, ${name}!`;

// Usage:
<Home name="John" />;

// Result:
Hello, John!
```
