# **Complete Reference: C23 `<stdbit.h>` Header File**

## **Introduction**

The **`<stdbit.h>`** header is a new addition in the **C23 standard** that provides comprehensive bit manipulation utilities. This header contains **4 macros** and **70 functions** (14 type-generic macros × 5 type-specific variants each) for working with unsigned integer bit patterns.

## **Endianness Detection Macros**

### **`__STDC_VERSION_STDBIT_H__`**

- **Type**: Version macro
- **Value**: `202311L`
- **Use**: Feature detection for C23 `stdbit.h` support
- **Example**:

```c
#if __STDC_VERSION_STDBIT_H__ >= 202311L
    // C23 stdbit.h is available
#endif
```

### **`__STDC_ENDIAN_LITTLE__`**

- **Type**: Endianness constant
- **Value**: Implementation-defined unique integer
- **Use**: Represents little-endian byte order
- **Example**:

```c
if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_LITTLE__) {
    printf("Little endian system\n");
}
```

### **`__STDC_ENDIAN_BIG__`**

- **Type**: Endianness constant
- **Value**: Implementation-defined unique integer
- **Use**: Represents big-endian byte order
- **Example**:

```c
if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_BIG__) {
    printf("Big endian system\n");
}
```

### **`__STDC_ENDIAN_NATIVE__`**

- **Type**: System endianness indicator
- **Value**: Equals `__STDC_ENDIAN_LITTLE__` or `__STDC_ENDIAN_BIG__`
- **Use**: Determines current system's byte order
- **Example**:

```c
if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_LITTLE__) {
    printf("Native is little endian\n");
} else if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_BIG__) {
    printf("Native is big endian\n"); 
} else {
    printf("Mixed endian system\n");
}
```

## **Bit Manipulation Functions**

All functions come in **5 variants** for different unsigned integer types:

- **`_uc`** → `unsigned char`
- **`_us`** → `unsigned short`
- **`_ui`** → `unsigned int`
- **`_ul`** → `unsigned long`
- **`_ull`** → `unsigned long long`

### **Leading Bit Functions**

#### **`stdc_leading_zeros`**

- **Return Type**: `unsigned int`
- **Use**: Count consecutive leading zero bits from MSB
- **Example**: `stdc_leading_zeros_ui(0x0F00)` → `20` (for 32-bit)
- **Variants**: `stdc_leading_zeros_uc/us/ui/ul/ull`

#### **`stdc_leading_ones`**

- **Return Type**: `unsigned int`
- **Use**: Count consecutive leading one bits from MSB
- **Example**: `stdc_leading_ones_ui(0xF0000000)` → `4`
- **Variants**: `stdc_leading_ones_uc/us/ui/ul/ull`

#### **`stdc_first_leading_zero`**

- **Return Type**: `unsigned int`
- **Use**: Find 1-based index of first leading zero bit
- **Example**: `stdc_first_leading_zero_us(0x7FFF)` → `1`
- **Variants**: `stdc_first_leading_zero_uc/us/ui/ul/ull`

#### **`stdc_first_leading_one`**

- **Return Type**: `unsigned int`
- **Use**: Find 1-based index of first leading one bit
- **Example**: `stdc_first_leading_one_us(0x0800)` → `5`
- **Variants**: `stdc_first_leading_one_uc/us/ui/ul/ull`

### **Trailing Bit Functions**

#### **`stdc_trailing_zeros`**

- **Return Type**: `unsigned int`
- **Use**: Count consecutive trailing zero bits from LSB
- **Example**: `stdc_trailing_zeros_uc(0x80)` → `7`
- **Output**: `0x7 0x8 0x0 0x40`
- **Variants**: `stdc_trailing_zeros_uc/us/ui/ul/ull`

#### **`stdc_trailing_ones`**

- **Return Type**: `unsigned int`
- **Use**: Count consecutive trailing one bits from LSB
- **Example**: `stdc_trailing_ones_us(0x9fff)` → `13`
- **Output**: `0x2 0xd 0x20 0x0`
- **Variants**: `stdc_trailing_ones_uc/us/ui/ul/ull`

#### **`stdc_first_trailing_zero`**

- **Return Type**: `unsigned int`
- **Use**: Find 1-based index of first trailing zero bit
- **Example**: `stdc_first_trailing_zero_us(0xa22b)` → `3`
- **Variants**: `stdc_first_trailing_zero_uc/us/ui/ul/ull`

#### **`stdc_first_trailing_one`**

- **Return Type**: `unsigned int`
- **Use**: Find 1-based index of first trailing one bit
- **Example**: `stdc_first_trailing_one_uc(0x08)` → `4`
- **Variants**: `stdc_first_trailing_one_uc/us/ui/ul/ull`

### **Bit Counting Functions**

#### **`stdc_count_zeros`**

- **Return Type**: `unsigned int`
- **Use**: Count total number of zero bits
- **Example**: `stdc_count_zeros_uc(0x42)` → `6` (for 8-bit)
- **Variants**: `stdc_count_zeros_uc/us/ui/ul/ull`

#### **`stdc_count_ones`** (Population Count)

- **Return Type**: `unsigned int`
- **Use**: Count total number of one bits
- **Example**: `stdc_count_ones_uc(0x42)` → `2`
- **Output**: `0x2 0x7 0x20 0x0`
- **Variants**: `stdc_count_ones_uc/us/ui/ul/ull`

### **Power-of-2 Functions**

#### **`stdc_has_single_bit`**

- **Return Type**: `bool`
- **Use**: Test if value is exact power of 2 (single bit set)
- **Example**: `stdc_has_single_bit_ui(0x08)` → `true`
- **Implementation**: `(__x ^ (__x - 1)) > __x - 1`
- **Variants**: `stdc_has_single_bit_uc/us/ui/ul/ull`

#### **`stdc_bit_width`**

- **Return Type**: `unsigned int`
- **Use**: Number of bits needed to represent value
- **Example**: `stdc_bit_width(5)` → `3` (binary `101` needs 3 bits)
- **Formula**: `1 + floor(log₂(x))` for x > 0, `0` for x = 0
- **Variants**: `stdc_bit_width_uc/us/ui/ul/ull`

#### **`stdc_bit_floor`**

- **Return Type**: Same as input type
- **Use**: Largest power of 2 not greater than value
- **Example**: `stdc_bit_floor(10)` → `8`
- **Implementation**: `x ? (T)1 << floor(log2(x)) : 0`
- **Variants**: `stdc_bit_floor_uc/us/ui/ul/ull`

#### **`stdc_bit_ceil`**

- **Return Type**: Same as input type
- **Use**: Smallest power of 2 not less than value
- **Example**: `stdc_bit_ceil(10)` → `16`
- **Implementation**: `x ? (T)1 << ceil(log2(x)) : 1`
- **Variants**: `stdc_bit_ceil_uc/us/ui/ul/ull`

## **Complete Example with Output**

```c
#include <stdbit.h>
#include <stdio.h>
#include <limits.h>

int main(void) {
    unsigned char uc_val = 0x42;     // 01000010 binary
    unsigned short us_val = 0xa22b;  // 1010001000101011 binary  
    unsigned int ui_val = 0x0F00;    // 00001111000000000000000000000000 binary
    
    printf("=== ENDIANNESS DETECTION ===\n");
    if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_LITTLE__) {
        printf("System is little endian\n");
    } else if (__STDC_ENDIAN_NATIVE__ == __STDC_ENDIAN_BIG__) {
        printf("System is big endian\n");
    }
    
    printf("\n=== BIT COUNTING FUNCTIONS ===\n");
    printf("stdc_count_ones_uc(0x42): %u\n", stdc_count_ones_uc(uc_val));
    printf("stdc_count_zeros_uc(0x42): %u\n", stdc_count_zeros_uc(uc_val));
    
    printf("\n=== POWER OF 2 TESTING ===\n");
    for (unsigned int val = 0; val <= 8; val++) {
        printf("Value %u: has_single_bit=%s, bit_width=%u, bit_ceil=%u\n", 
               val,
               stdc_has_single_bit_ui(val) ? "true" : "false",
               stdc_bit_width_ui(val),
               stdc_bit_ceil_ui(val));
    }
    
    return 0;
}
```

**Expected Output:**

```c
=== ENDIANNESS DETECTION ===
System is little endian

=== BIT COUNTING FUNCTIONS ===
stdc_count_ones_uc(0x42): 2
stdc_count_zeros_uc(0x42): 6

=== POWER OF 2 TESTING ===
Value 0: has_single_bit=false, bit_width=0, bit_ceil=1
Value 1: has_single_bit=true, bit_width=1, bit_ceil=1
Value 2: has_single_bit=true, bit_width=2, bit_ceil=2
Value 3: has_single_bit=false, bit_width=2, bit_ceil=4
Value 4: has_single_bit=true, bit_width=3, bit_ceil=4
Value 5: has_single_bit=false, bit_width=3, bit_ceil=8
Value 6: has_single_bit=false, bit_width=3, bit_ceil=8
Value 7: has_single_bit=false, bit_width=3, bit_ceil=8
Value 8: has_single_bit=true, bit_width=4, bit_ceil=8
```

## **Key Implementation Details**

**Function Availability**: All 70 specific functions plus 14 type-generic macros

**Type Support**: Works only with unsigned integer types (`unsigned char`, `unsigned short`, `unsigned int`, `unsigned long`, `unsigned long long`)

**Return Types**: Most functions return `unsigned int`, except:

- `stdc_has_single_bit` returns `bool`
- `stdc_bit_floor` and `stdc_bit_ceil` return same type as input

**Compiler Support**: Requires C23 standard (`-std=c23`) with GCC 13+, Clang 16+

**Error Handling**: Functions cannot fail and always return valid results

**Performance**: Many implementations use efficient CPU instructions like `lzcnt`, `tzcnt`, `popcnt`

***

The `<stdbit.h>` header represents a significant advancement in C's bit manipulation capabilities, providing standardized, efficient functions for common bit operations that were previously implemented using compiler-specific intrinsics or manual bit manipulation.

***
