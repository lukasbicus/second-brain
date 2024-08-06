---
title: Validation
summary: Benefits of validation, list of popular libraries. Introduction to Zod library.
slug: an-slug
project-tags: 
  - bysquare
  - self education
thematic-tags:
  - validation
  - zod
---

# Validation

*Benefits of validation, list of popular libraries. Introduction to Zod library.*

---
the content

## The importance of object validation
- the process of validations brings benefits to the applications[^1]
1. Data consistency
   - validation ensures data consistency (f.e. before storing data in database)
2. Error avoidance
   - validation prevents runtime errors, exceptions and unexpected behaviour caused by invalid data

## Best practices
- setup validation methods to suit your application's specific data requirements
- handle validation errors gracefully, providing user-friendly feedback and log meaningful error messages
- consider performance, especially when dealing with large objects

## Validation libraries

Here are some validation libraries, I met along. Each will add complexity to your project, so it's best to use only one at time.

Ajv: robust and performant library for validation. The most used.

Joi: Joi is a powerful schema description language and data validator for JavaScript. It's often used to validate data in REST APIs and can validate both simple and complex data structures.

Yup: Yup is a JavaScript schema builder for value parsing and validation. It's leaner than Joi and is often used with form libraries like Formik for front-end validation in React applications.

Zod: Zod[^2] is a TypeScript-first schema declaration and validation library. It allows you to create schemas using TypeScript types and provides great type inference.

## Zod[^2]
I've picked Zod
- it's small
- has no dependencies
- provides great type inference

Type inference refers to Zod's ability to automatically deduce and provide TypeScript types based on the schema definitions you create.

That means Zod provides not only runtime validation, but also compile time type safety.

```typescript
import { z } from 'zod';

const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
  email: z.string().email(),
  isAdmin: z.boolean().optional(),
});

// Zod infers the type of `User` based on `UserSchema`
type User = z.infer<typeof UserSchema>;

// The inferred `User` type is equivalent to:
// type User = {
//   id: number;
//   name: string;
//   email: string;
//   isAdmin?: boolean;
// };
```

## How to use Zod

Zod empowers you to create basic schemas. More complex object can be validated by composing simple schemas.

```typescript
const BICSchema = z.string().regex(
  /^(?=[A-Z]{6}[A-Z0-9]{2}(?:[A-Z0-9]{3})?$)/,
  "Invalid BIC format",
);
const IBANSchema = z.string()
  .min(15, "IBAN must be at least 15 characters long")
  .max(34, "IBAN cannot be more than 34 characters long")
  .regex(
    /^[A-Z]{2}[0-9A-Z]{13,32}$/,
    "IBAN must start with two uppercase letters followed by up to 32 alphanumeric characters",
  );

// composed schema, that represents typescript type: {iban: string; bic?: string}
const BankAccountSchema = z.object({
   iban: IBANSchema,
   bic: BICSchema.optional(),
});

const result = BankAccountSchema.parse({
   iban: "LC14BOSL123456789012345678901234"
}) // results in LC14BOSL123456789012345678901234
BankAccountSchema.parse({
   iban: "Invalid iban"
}) // will throw a zod error
```

If you are interested in Zod, try it in [playground](https://zod-playground.vercel.app/)

## Error handling
Zod is able to specify path to node in validated object, where validation fails. Check [Error handling in forms](https://zod.dev/ERROR_HANDLING?id=error-handling-for-forms)

```typescript
const FormData = z.object({
  name: z.string(),
  contactInfo: z.object({
    email: z.string().email(),
    phone: z.string().optional(),
  }),
});
const result = FormData.safeParse({
   name: null,
   contactInfo: {
      email: "not an email",
      phone: "867-5309",
   },
});
if (!result.success) {
   console.log(result.error.issues);
}
/*
  [
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "null",
      "path": ["name"],
      "message": "Expected string, received null"
    },
    {
      "validation": "email",
      "code": "invalid_string",
      "message": "Invalid email",
      "path": ["contactInfo","email"]
    }
  ]
*/
```

---
### Sources

[^1]: [Object validation in javascript best practices and examples](https://medium.com/@stheodorejohn/object-validation-in-javascript-best-practices-and-examples-112855955566)
[^2]: [Zod](https://zod.dev/)
