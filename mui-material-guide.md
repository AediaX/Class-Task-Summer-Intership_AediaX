# MUI (Material UI) — Complete Guide for React

## 1. What is MUI?

**MUI (Material UI)** is a React component library that implements Google's **Material Design** system. It gives you pre-built, production-ready components (buttons, cards, forms, dialogs, tables, etc.) so you don't have to build UI from scratch.

- Package name: `@mui/material`
- Icons package: `@mui/icons-material`
- Styling engine: `@emotion/react` + `@emotion/styled` (used internally)
- Website: mui.com

### Why use MUI?
- Consistent, accessible, responsive components out of the box
- Built-in theming (dark/light mode, custom color palettes)
- Powerful `sx` prop for quick inline styling (like Tailwind, but JS-object based)
- Great with `.jsx` / `.tsx` React projects

---

## 2. Installation

```bash
npm install @mui/material @emotion/react @emotion/styled
```

For icons:

```bash
npm install @mui/icons-material
```

For Google's Roboto font (recommended):

```bash
npm install @fontsource/roboto
```

Then import it once in your entry file (e.g. `main.jsx` / `index.jsx`):

```jsx
import '@fontsource/roboto/300.css';
import '@fontsource/roboto/400.css';
import '@fontsource/roboto/500.css';
import '@fontsource/roboto/700.css';
```

---

## 3. Basic Usage in `.jsx`

```jsx
import React from 'react';
import Button from '@mui/material/Button';

function App() {
  return (
    <div>
      <Button variant="contained" color="primary">
        Click Me
      </Button>
    </div>
  );
}

export default App;
```

You can also import multiple components from the root package (slightly larger bundle unless tree-shaking is configured):

```jsx
import { Button, TextField, Card } from '@mui/material';
```

---

## 4. The `sx` Prop — MUI's Styling Superpower

The `sx` prop lets you write CSS-like styles directly as a **JavaScript object** on almost any MUI component. It supports:
- Normal CSS properties (camelCase)
- Theme-aware shorthand values (spacing, colors, breakpoints)
- Pseudo-selectors (`:hover`, `:focus`)
- Responsive breakpoints

### Basic Syntax

```jsx
<Box
  sx={{
    width: 200,
    height: 100,
    backgroundColor: 'primary.main',
    color: 'white',
    borderRadius: 2,
    padding: 3,
    margin: '10px auto',
    textAlign: 'center',
  }}
>
  Hello MUI
</Box>
```

### Key `sx` Property Behaviors

| Property | What it accepts | Example |
|---|---|---|
| `p`, `pt`, `pb`, `pl`, `pr`, `px`, `py` | spacing units (theme spacing = 8px * value) | `p: 2` → 16px padding |
| `m`, `mt`, `mb`, `ml`, `mr`, `mx`, `my` | spacing units | `mt: 4` → 32px margin-top |
| `width`, `height` | number (px) or string (`%`, `vh`, etc.) | `width: 300` or `width: '100%'` |
| `bgcolor` / `backgroundColor` | theme color path or CSS color | `bgcolor: 'secondary.light'` |
| `color` | theme color path or CSS color | `color: 'text.secondary'` |
| `border` | CSS border string | `border: '1px solid #ccc'` |
| `borderRadius` | number (theme shape unit) or string | `borderRadius: 2` |
| `boxShadow` | number (elevation 0–24) or CSS string | `boxShadow: 3` |
| `display` | CSS display value | `display: 'flex'` |
| `flexDirection`, `justifyContent`, `alignItems` | flexbox values | `justifyContent: 'center'` |
| `gap` | spacing units | `gap: 2` |
| `fontSize`, `fontWeight` | number/string or theme typography | `fontSize: '1.2rem'` |
| `'&:hover'` | nested pseudo-class styles | see below |

### Spacing Shorthand Explained
By default `theme.spacing(1) = 8px`. So:
```js
p: 1   // padding: 8px
p: 2   // padding: 16px
p: 3   // padding: 24px
```

### Theme Color Paths
Instead of hardcoding hex codes, reference the theme:
```jsx
sx={{ color: 'primary.main' }}      // theme.palette.primary.main
sx={{ bgcolor: 'error.light' }}     // theme.palette.error.light
sx={{ color: 'text.secondary' }}    // theme.palette.text.secondary
```

### Pseudo-classes and Nested Selectors
```jsx
<Button
  sx={{
    backgroundColor: 'primary.main',
    '&:hover': {
      backgroundColor: 'primary.dark',
    },
    '& .MuiButton-label': {
      textTransform: 'none',
    },
  }}
>
  Hover Me
</Button>
```

### Responsive Styles (Breakpoints)
```jsx
<Box
  sx={{
    width: {
      xs: '100%',   // mobile
      sm: '80%',    // small tablets
      md: '60%',    // tablets/small laptops
      lg: '40%',    // desktops
      xl: '30%',    // large screens
    },
    fontSize: { xs: 14, md: 18 },
  }}
>
  Responsive Box
</Box>
```

### Array Syntax (alternative responsive shorthand)
```jsx
<Box sx={{ width: [100, 200, 300, 400] }} /> // xs, sm, md, lg in order
```

### Function-based `sx` (access theme directly)
```jsx
<Box
  sx={(theme) => ({
    color: theme.palette.mode === 'dark' ? '#fff' : '#000',
    padding: theme.spacing(2),
  })}
/>
```

---

## 5. Other Ways to Style MUI Components

| Method | Use case |
|---|---|
| `sx` prop | Quick, one-off, component-level styles (most common) |
| `styled()` API | Reusable styled components (like styled-components) |
| `theme` customization | Global design tokens (colors, typography, spacing) |
| `className` + CSS/SCSS | Traditional CSS files |
| `useTheme()` hook | Access theme values inside component logic |

### `styled()` Example
```jsx
import { styled } from '@mui/material/styles';
import Button from '@mui/material/Button';

const StyledButton = styled(Button)(({ theme }) => ({
  backgroundColor: theme.palette.primary.main,
  borderRadius: 12,
  padding: theme.spacing(1, 3),
  '&:hover': {
    backgroundColor: theme.palette.primary.dark,
  },
}));

// Usage
<StyledButton variant="contained">Styled Button</StyledButton>
```

### Custom Theme Example
```jsx
import { createTheme, ThemeProvider } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: { main: '#1976d2' },
    secondary: { main: '#9c27b0' },
    mode: 'light', // or 'dark'
  },
  typography: {
    fontFamily: 'Roboto, sans-serif',
  },
  shape: {
    borderRadius: 8,
  },
});

function App() {
  return (
    <ThemeProvider theme={theme}>
      {/* your app */}
    </ThemeProvider>
  );
}
```

---

## 6. Commonly Used MUI Components (with props & values)

### 6.1 `Button`
```jsx
import Button from '@mui/material/Button';

<Button
  variant="contained"      // 'text' | 'outlined' | 'contained'
  color="primary"          // 'primary' | 'secondary' | 'success' | 'error' | 'info' | 'warning'
  size="medium"             // 'small' | 'medium' | 'large'
  disabled={false}
  startIcon={<SaveIcon />}
  endIcon={<SendIcon />}
  onClick={() => console.log('clicked')}
  sx={{ borderRadius: 3, textTransform: 'none' }}
>
  Submit
</Button>
```

### 6.2 `TextField`
```jsx
import TextField from '@mui/material/TextField';

<TextField
  label="Email"
  variant="outlined"       // 'outlined' | 'filled' | 'standard'
  color="primary"
  size="small"              // 'small' | 'medium'
  fullWidth
  required
  error={false}
  helperText="Enter a valid email"
  type="email"
  value={email}
  onChange={(e) => setEmail(e.target.value)}
  sx={{ mb: 2 }}
/>
```

### 6.3 `Box` (generic styling wrapper, like a `div`)
```jsx
import Box from '@mui/material/Box';

<Box
  component="section"
  sx={{ display: 'flex', flexDirection: 'column', gap: 2, p: 3 }}
>
  Content here
</Box>
```

### 6.4 `Grid` (layout system)
```jsx
import Grid from '@mui/material/Grid';

<Grid container spacing={2}>
  <Grid item xs={12} sm={6} md={4}>
    <Box sx={{ bgcolor: 'lightblue', p: 2 }}>Item 1</Box>
  </Grid>
  <Grid item xs={12} sm={6} md={4}>
    <Box sx={{ bgcolor: 'lightgreen', p: 2 }}>Item 2</Box>
  </Grid>
</Grid>
```
- `container`: marks a Grid as a flex container
- `item`: marks a Grid as a flex item
- `xs`, `sm`, `md`, `lg`, `xl`: number of columns (out of 12) at each breakpoint
- `spacing`: gap between grid items (theme spacing units)

### 6.5 `Stack` (simple flex layout)
```jsx
import Stack from '@mui/material/Stack';

<Stack direction="row" spacing={2} alignItems="center" justifyContent="space-between">
  <Button>One</Button>
  <Button>Two</Button>
</Stack>
```
- `direction`: `'row' | 'column' | 'row-reverse' | 'column-reverse'`
- `spacing`: gap value (theme units)
- `divider`: element rendered between children

### 6.6 `Card`
```jsx
import { Card, CardContent, CardActions, CardMedia, Typography } from '@mui/material';

<Card sx={{ maxWidth: 345 }}>
  <CardMedia component="img" height="140" image="/img.jpg" alt="desc" />
  <CardContent>
    <Typography variant="h5">Title</Typography>
    <Typography variant="body2" color="text.secondary">
      Some description text
    </Typography>
  </CardContent>
  <CardActions>
    <Button size="small">Learn More</Button>
  </CardActions>
</Card>
```

### 6.7 `Typography`
```jsx
import Typography from '@mui/material/Typography';

<Typography
  variant="h4"        // 'h1'...'h6', 'subtitle1', 'subtitle2', 'body1', 'body2', 'caption', 'button', 'overline'
  color="text.primary"
  align="center"       // 'left' | 'center' | 'right' | 'justify'
  gutterBottom
  sx={{ fontWeight: 600 }}
>
  Heading Text
</Typography>
```

### 6.8 `AppBar` + `Toolbar` (navigation header)
```jsx
import { AppBar, Toolbar, Typography, IconButton } from '@mui/material';
import MenuIcon from '@mui/icons-material/Menu';

<AppBar position="static" color="primary">
  <Toolbar>
    <IconButton edge="start" color="inherit">
      <MenuIcon />
    </IconButton>
    <Typography variant="h6" sx={{ flexGrow: 1 }}>
      My App
    </Typography>
  </Toolbar>
</AppBar>
```

### 6.9 `Dialog` (modal)
```jsx
import { Dialog, DialogTitle, DialogContent, DialogActions, Button } from '@mui/material';

<Dialog open={open} onClose={handleClose} fullWidth maxWidth="sm">
  <DialogTitle>Confirm Action</DialogTitle>
  <DialogContent>Are you sure you want to proceed?</DialogContent>
  <DialogActions>
    <Button onClick={handleClose}>Cancel</Button>
    <Button onClick={handleConfirm} variant="contained">Confirm</Button>
  </DialogActions>
</Dialog>
```

### 6.10 `Select` (dropdown)
```jsx
import { Select, MenuItem, FormControl, InputLabel } from '@mui/material';

<FormControl fullWidth size="small">
  <InputLabel>Category</InputLabel>
  <Select
    value={category}
    label="Category"
    onChange={(e) => setCategory(e.target.value)}
  >
    <MenuItem value="tech">Tech</MenuItem>
    <MenuItem value="design">Design</MenuItem>
  </Select>
</FormControl>
```

### 6.11 `Checkbox` / `Switch` / `Radio`
```jsx
import { Checkbox, Switch, Radio, FormControlLabel } from '@mui/material';

<FormControlLabel control={<Checkbox checked={checked} onChange={handleChange} />} label="Accept Terms" />
<FormControlLabel control={<Switch checked={dark} onChange={toggleDark} />} label="Dark Mode" />
<FormControlLabel control={<Radio checked={selected} value="a" />} label="Option A" />
```

### 6.12 `Table`
```jsx
import { Table, TableHead, TableBody, TableRow, TableCell, TableContainer, Paper } from '@mui/material';

<TableContainer component={Paper}>
  <Table>
    <TableHead>
      <TableRow>
        <TableCell>Name</TableCell>
        <TableCell align="right">Score</TableCell>
      </TableRow>
    </TableHead>
    <TableBody>
      <TableRow>
        <TableCell>Amresh</TableCell>
        <TableCell align="right">95</TableCell>
      </TableRow>
    </TableBody>
  </Table>
</TableContainer>
```

### 6.13 `Alert` / `Snackbar`
```jsx
import { Alert, Snackbar } from '@mui/material';

<Alert severity="success" onClose={() => {}}>
  Saved successfully!
</Alert>

<Snackbar
  open={open}
  autoHideDuration={3000}
  onClose={handleClose}
  message="Changes saved"
/>
```
- `severity`: `'error' | 'warning' | 'info' | 'success'`

### 6.14 `Avatar`, `Chip`, `Badge`
```jsx
import { Avatar, Chip, Badge } from '@mui/material';

<Avatar src="/user.jpg" alt="User" sx={{ width: 56, height: 56 }} />
<Chip label="React" color="primary" onDelete={() => {}} />
<Badge badgeContent={4} color="error">
  <MailIcon />
</Badge>
```

---

## 7. Common `sx` Value Types Cheat Sheet

| Value type | Example | Notes |
|---|---|---|
| Number (spacing) | `p: 2` | Multiplied by `theme.spacing()` (default 8px) |
| Raw px/string | `width: '250px'` | Used as-is |
| Theme color path | `color: 'primary.main'` | Resolves through `theme.palette` |
| CSS color | `color: '#ff5722'` | Used as-is |
| Object per breakpoint | `{ xs: 1, md: 3 }` | Responsive value |
| Function | `(theme) => ({...})` | Full access to theme object |
| Elevation number | `boxShadow: 4` | Maps to MUI shadow scale (0–24) |

---

## 8. Quick Reference: Import Cheat Sheet

```jsx
import Button from '@mui/material/Button';
import TextField from '@mui/material/TextField';
import Box from '@mui/material/Box';
import Grid from '@mui/material/Grid';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import Card from '@mui/material/Card';
import CardContent from '@mui/material/CardContent';
import AppBar from '@mui/material/AppBar';
import Toolbar from '@mui/material/Toolbar';
import Dialog from '@mui/material/Dialog';
import Select from '@mui/material/Select';
import MenuItem from '@mui/material/MenuItem';
import Checkbox from '@mui/material/Checkbox';
import Switch from '@mui/material/Switch';
import Table from '@mui/material/Table';
import Alert from '@mui/material/Alert';
import Snackbar from '@mui/material/Snackbar';
import Avatar from '@mui/material/Avatar';
import Chip from '@mui/material/Chip';
import Badge from '@mui/material/Badge';
import { ThemeProvider, createTheme, styled, useTheme } from '@mui/material/styles';
```

---

## 9. Summary

- **MUI** = Material Design components for React.
- Style with the **`sx` prop** for quick, theme-aware, responsive CSS-in-JS.
- Use **`styled()`** for reusable, named styled components.
- Use **`createTheme` + `ThemeProvider`** to set global design tokens (colors, typography, spacing, dark/light mode).
- Almost every component accepts `variant`, `color`, `size` props plus `sx` for fine-tuned styling.
