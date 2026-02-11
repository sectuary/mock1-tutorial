# Why Does It Start with key0?

## Your Question

**"Why do we start with key0?"**

## Short Answer

Because **Sunday = 0** in C programming!

## The C Standard: struct tm

In C, the `tm_wday` field represents day of week:

```c
struct tm {
    int tm_wday;     // day of week (0-6)
    //                  0 = Sunday!
    //                  1 = Monday
    //                  2 = Tuesday
    //                  ...
    //                  6 = Saturday
};
```

## The Mapping

| tm_wday | Day | Key File |
|---------|-----|----------|
| 0 | Sunday | key0 |
| 1 | Monday | key1 |
| 2 | Tuesday | key2 |
| 3 | Wednesday | key3 |
| 4 | Thursday | key4 |
| 5 | Friday | key5 |
| 6 | Saturday | key6 |

## In the Code

```c
wkday = localtime(&rawtime)->tm_wday;

switch (wkday) {
    case 0:  // Sunday
        key_file_use = "key0";
        break;
    case 1:  // Monday
        key_file_use = "key1";
        break;
    // ... up to case 6
}
```

## Why key7 Is Never Used

Because `tm_wday` only has values 0-6 (7 days).

There is NO day 7!

## Settings.ini Example

- Created: Friday, February 5, 2016
- Friday = tm_wday value 5
- Therefore: **key5** is used

## Key Takeaway

The program doesn't "choose" to start at 0.

It MUST match how C defines days: **Sunday = 0**

This is a C programming standard, not the programmer's choice!
