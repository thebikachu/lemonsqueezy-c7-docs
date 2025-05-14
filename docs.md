Here are the generated docs:

TITLE: Setup Lemon Squeezy Client
DESCRIPTION: Initializes the Lemon Squeezy client with an API key and an optional error handling callback. This function should be called once when your application starts.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/internal/setup/index.ts#L10-L14
LANGUAGE: ts
CODE:

export function lemonSqueezySetup(config: Config) {
  const { apiKey, onError } = config;
  setKV(CONFIG_KEY, { apiKey, onError });
  return config;
}


TITLE: Create a Checkout
DESCRIPTION: Create a checkout. This function takes a store ID, variant ID, and an optional checkout object to customize the checkout session, including custom pricing, product options, and prefilled data.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/checkouts/index.ts#L26-L84
LANGUAGE: ts
CODE:

export function createCheckout(
  storeId: number | string,
  variantId: number | string,
  checkout: NewCheckout = {}
) {
  requiredCheck({ storeId, variantId });

  const {
    customPrice,
    productOptions,
    checkoutOptions,
    checkoutData,
    expiresAt,
    preview,
    testMode,
  } = checkout;

  const relationships = {
    store: {
      data: {
        type: "stores",
        id: storeId.toString(),
      },
    },
    variant: {
      data: {
        type: "variants",
        id: variantId.toString(),
      },
    },
  };

  const attributes = {
    customPrice,
    expiresAt,
    preview,
    testMode,
    productOptions,
    checkoutOptions,
    checkoutData: {
      ...checkoutData,
      variantQuantities: checkoutData?.variantQuantities?.map((item) =>
        convertKeys(item)
      ),
    },
  };

  return $fetch<Checkout>({
    path: "/v1/checkouts",
    method: "POST",
    body: {
      data: {
        type: "checkouts",
        attributes: convertKeys(attributes),
        relationships,
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Checkout
DESCRIPTION: Retrieve a checkout by its ID. Optionally, related resources can be included in the response.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/checkouts/index.ts#L94-L102
LANGUAGE: ts
CODE:

export function getCheckout(
  checkoutId: number | string,
  params: GetCheckoutParams = {}
) {
  requiredCheck({ checkoutId });
  return $fetch<Checkout>({
    path: `/v1/checkouts/${checkoutId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Checkouts
DESCRIPTION: List all checkouts. This function allows pagination and filtering by store ID or variant ID. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/checkouts/index.ts#L117-L121
LANGUAGE: ts
CODE:

export function listCheckouts(params: ListCheckoutsParams = {}) {
  return $fetch<ListCheckouts>({
    path: `/v1/checkouts${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Create a Customer
DESCRIPTION: Create a customer for a given store. Requires store ID, customer name, and email. Optional details include city, region, and country.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/customers/index.ts#L28-L51
LANGUAGE: ts
CODE:

export function createCustomer(
  storeId: number | string,
  customer: NewCustomer
) {
  requiredCheck({ storeId });
  return $fetch<Customer>({
    path: "/v1/customers",
    method: "POST",
    body: {
      data: {
        type: "customers",
        attributes: customer,
        relationships: {
          store: {
            data: {
              type: "stores",
              id: storeId.toString(),
            },
          },
        },
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Update a Customer
DESCRIPTION: Update a customer's details. This can include their name, email, city, region, country, or status (e.g., to archive them).
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/customers/index.ts#L66-L82
LANGUAGE: ts
CODE:

export function updateCustomer(
  customerId: string | number,
  customer: UpdateCustomer
) {
  requiredCheck({ customerId });
  return $fetch<Customer>({
    path: `/v1/customers/${customerId}`,
    method: "PATCH",
    body: {
      data: {
        type: "customers",
        id: customerId.toString(),
        attributes: customer,
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Archive a Customer
DESCRIPTION: Archive a customer by setting their status to 'archived'.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/customers/index.ts#L90-L105
LANGUAGE: ts
CODE:

export function archiveCustomer(customerId: string | number) {
  requiredCheck({ customerId });
  return $fetch<Customer>({
    path: `/v1/customers/${customerId}`,
    method: "PATCH",
    body: {
      data: {
        type: "customers",
        id: customerId.toString(),
        attributes: {
          status: "archived",
        },
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Customer
DESCRIPTION: Retrieve a customer by their ID. Optionally, related resources can be included in the response.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/customers/index.ts#L115-L123
LANGUAGE: ts
CODE:

export function getCustomer(
  customerId: string | number,
  params: GetCustomerParams = {}
) {
  requiredCheck({ customerId });
  return $fetch<Customer>({
    path: `/v1/customers/${customerId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Customers
DESCRIPTION: List all customers. This function allows pagination and filtering by store ID or email. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/customers/index.ts#L138-L142
LANGUAGE: ts
CODE:

export function listCustomers(params: ListCustomersParams = {}) {
  return $fetch<ListCustomers>({
    path: `/v1/customers${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Discount Redemption
DESCRIPTION: Retrieve a discount redemption by its ID. Optionally, related resources can be included in the response.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discountRedemptions/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getDiscountRedemption(
  discountRedemptionId: number | string,
  params: GetDiscountRedemptionParams = {}
) {
  requiredCheck({ discountRedemptionId });
  return $fetch<DiscountRedemption>({
    path: `/v1/discount-redemptions/${discountRedemptionId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Discount Redemptions
DESCRIPTION: List all discount redemptions. This function allows pagination and filtering by discount ID or order ID. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discountRedemptions/index.ts#L45-L51
LANGUAGE: ts
CODE:

export function listDiscountRedemptions(
  params: ListDiscountRedemptionsParams = {}
) {
  return $fetch<ListDiscountRedemptions>({
    path: `/v1/discount-redemptions${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Create a Discount
DESCRIPTION: Create a discount with specified parameters including store ID, name, code, amount, and other limitations or duration settings.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discounts/index.ts#L23-L87
LANGUAGE: ts
CODE:

export function createDiscount(discount: NewDiscount) {
  const {
    storeId,
    variantIds,
    name,
    amount,
    amountType = "fixed",
    code = generateDiscount(),
    isLimitedToProducts = false,
    isLimitedRedemptions = false,
    maxRedemptions = 0,
    startsAt = null,
    expiresAt = null,
    duration = "once",
    durationInMonths = 1,
    testMode,
  } = discount;

  requiredCheck({ storeId, name, code, amount });

  const attributes = convertKeys({
    name,
    amount,
    amountType,
    code,
    isLimitedRedemptions,
    isLimitedToProducts,
    maxRedemptions,
    startsAt,
    expiresAt,
    duration,
    durationInMonths,
    testMode,
  });

  const relationships: Record<string, any> = {
    store: {
      data: {
        type: "stores",
        id: storeId.toString(),
      },
    },
  };

  if (variantIds && variantIds.length > 0) {
    relationships.variants = {
      data: variantIds.map((id) => ({
        type: "variants",
        id: id.toString(),
      })),
    };
  }

  return $fetch<Discount>({
    path: "/v1/discounts",
    method: "POST",
    body: {
      data: {
        type: "discounts",
        attributes,
        relationships,
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Discounts
DESCRIPTION: List all discounts. This function allows pagination and filtering by store ID. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discounts/index.ts#L101-L105
LANGUAGE: ts
CODE:

export function listDiscounts(params: ListDiscountsParams = {}) {
  return $fetch<ListDiscounts>({
    path: `/v1/discounts${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Discount
DESCRIPTION: Retrieve a discount by its ID. Optionally, related resources can be included in the response.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discounts/index.ts#L115-L123
LANGUAGE: ts
CODE:

export function getDiscount(
  discountId: number | string,
  params: GetDiscountParams = {}
) {
  requiredCheck({ discountId });
  return $fetch<Discount>({
    path: `/v1/discounts/${discountId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Delete a Discount
DESCRIPTION: Delete a discount by its ID.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/discounts/index.ts#L131-L137
LANGUAGE: ts
CODE:

export function deleteDiscount(discountId: string | number) {
  requiredCheck({ discountId });
  return $fetch<null>({
    path: `/v1/discounts/${discountId}`,
    method: "DELETE",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a File
DESCRIPTION: Retrieve a file by its ID. Optionally, related resources such as the variant it belongs to can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/files/index.ts#L17-L22
LANGUAGE: ts
CODE:

export function getFile(fileId: number | string, params: GetFileParams = {}) {
  requiredCheck({ fileId });
  return $fetch<File>({
    path: `/v1/files/${fileId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Files
DESCRIPTION: List all files. This function allows pagination and filtering by variant ID. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/files/index.ts#L36-L40
LANGUAGE: ts
CODE:

export function listFiles(params: ListFilesParams = {}) {
  return $fetch<ListFiles>({
    path: `/v1/files${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Activate a License Key
DESCRIPTION: Activate a license key with a given instance name.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/license/index.ts#L15-L28
LANGUAGE: ts
CODE:

export async function activateLicense(
  licenseKey: string,
  instanceName: string
) {
  requiredCheck({ licenseKey, instanceName });
  return $fetch<ActivateLicense>(
    {
      path: "/v1/licenses/activate",
      method: "POST",
      body: convertKeys({ licenseKey, instanceName }),
    },
    false
  );
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Validate a License Key
DESCRIPTION: Validate a license key. Optionally, an instance ID can be provided to validate a specific license key instance.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/license/index.ts#L37-L50
LANGUAGE: ts
CODE:

export async function validateLicense(licenseKey: string, instanceId?: string) {
  requiredCheck({ licenseKey });
  return $fetch<ValidateLicense>(
    {
      path: "/v1/licenses/validate",
      method: "POST",
      body: convertKeys({
        licenseKey,
        instanceId,
      }),
    },
    false
  );
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Deactivate a License Key Instance
DESCRIPTION: Deactivate a license key instance using the license key and the instance ID.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/license/index.ts#L59-L75
LANGUAGE: ts
CODE:

export async function deactivateLicense(
  licenseKey: string,
  instanceId: string
) {
  requiredCheck({ licenseKey, instanceId });
  return $fetch<DeactivateLicense>(
    {
      path: "/v1/licenses/deactivate",
      method: "POST",
      body: convertKeys({
        licenseKey,
        instanceId,
      }),
    },
    false
  );
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a License Key Instance
DESCRIPTION: Retrieve a license key instance by its ID. Optionally, related resources like the license key can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/licenseKeyInstances/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getLicenseKeyInstance(
  licenseKeyInstanceId: number | string,
  params: GetLicenseKeyInstanceParams = {}
) {
  requiredCheck({ licenseKeyInstanceId });
  return $fetch<LicenseKeyInstance>({
    path: `/v1/license-key-instances/${licenseKeyInstanceId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List License Key Instances
DESCRIPTION: List all license key instances. This function allows pagination and filtering by license key ID. Related resources can also be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/licenseKeyInstances/index.ts#L44-L50
LANGUAGE: ts
CODE:

export function listLicenseKeyInstances(
  params: ListLicenseKeyInstancesParams = {}
) {
  return $fetch<ListLicenseKeyInstances>({
    path: `/v1/license-key-instances${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a License Key
DESCRIPTION: Retrieve a license key by its ID. Optionally, related resources can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/licenseKeys/index.ts#L24-L32
LANGUAGE: ts
CODE:

export function getLicenseKey(
  licenseKeyId: number | string,
  params: GetLicenseKeyParams = {}
) {
  requiredCheck({ licenseKeyId });
  return $fetch<LicenseKey>({
    path: `/v1/license-keys/${licenseKeyId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List License Keys
DESCRIPTION: List all license keys. Allows filtering by store ID, order ID, order item ID, or product ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/licenseKeys/index.ts#L49-L53
LANGUAGE: ts
CODE:

export function listLicenseKeys(params: ListLicenseKeysParams = {}) {
  return $fetch<ListLicenseKeys>({
    path: `/v1/license-keys${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Update a License Key
DESCRIPTION: Update a license key's attributes such as activation limit, expiration date, or disabled status.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/licenseKeys/index.ts#L65-L85
LANGUAGE: ts
CODE:

export function updateLicenseKey(
  licenseKeyId: string | number,
  licenseKey: UpdateLicenseKey
) {
  requiredCheck({ licenseKeyId });

  const { activationLimit, disabled = false, expiresAt } = licenseKey;
  const attributes = convertKeys({ activationLimit, disabled, expiresAt });

  return $fetch<LicenseKey>({
    path: `/v1/license-keys/${licenseKeyId}`,
    method: "PATCH",
    body: {
      data: {
        type: "license-keys",
        id: licenseKeyId.toString(),
        attributes,
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve an Order Item
DESCRIPTION: Retrieve an order item by its ID. Optionally, related resources like order, product, or variant can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orderItems/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getOrderItem(
  orderItemId: number | string,
  params: GetOrderItemParams = {}
) {
  requiredCheck({ orderItemId });
  return $fetch<OrderItem>({
    path: `/v1/order-items/${orderItemId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Order Items
DESCRIPTION: List all order items. Allows filtering by order ID, product ID, or variant ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orderItems/index.ts#L46-L50
LANGUAGE: ts
CODE:

export function listOrderItems(params: ListOrderItemsParams = {}) {
  return $fetch<ListOrderItems>({
    path: `/v1/order-items${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve an Order
DESCRIPTION: Retrieve an order by its ID. Optionally, related resources like store, customer, or order items can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orders/index.ts#L25-L34
LANGUAGE: ts
CODE:

export function getOrder(
  orderId: number | string,
  params: GetOrderParams = {}
) {
  requiredCheck({ orderId });

  return $fetch<Order>({
    path: `/v1/orders/${orderId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Orders
DESCRIPTION: List all orders. Allows filtering by store ID or user email. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orders/index.ts#L49-L53
LANGUAGE: ts
CODE:

export function listOrders(params: ListOrdersParams = {}) {
  return $fetch<ListOrders>({
    path: `/v1/orders${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Generate Order Invoice PDF
DESCRIPTION: Generate a PDF invoice for a specific order. Requires order ID and customer details (name, address, etc.). Optional notes and locale can be provided. Returns a link to download the invoice.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orders/index.ts#L70-L85
LANGUAGE: ts
CODE:

export function generateOrderInvoice(
  orderId: number | string,
  params: GenerateOrderInvoiceParams
) {
  requiredCheck({ orderId });
  const searchParams = convertKeys(params);
  const queryString = new URLSearchParams(
    searchParams as Record<string, any>
  ).toString();
  const query = queryString ? `?${queryString}` : "";

  return $fetch<OrderInvoice>({
    path: `/v1/orders/${orderId}/generate-invoice${query}`,
    method: "POST",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Issue Order Refund
DESCRIPTION: Issues a partial refund for an order. Requires the order ID and the amount (in cents) to refund.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/orders/index.ts#L93-L109
LANGUAGE: ts
CODE:

export function issueOrderRefund(orderId: number | string, amount: number) {
  requiredCheck({ orderId, amount });

  const attributes = { amount };

  return $fetch<Order>({
    path: `/v1/orders/${orderId}/refund`,
    method: "POST",
    body: {
      data: {
        type: "orders",
        id: orderId.toString(),
        attributes: convertKeys(attributes),
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Price
DESCRIPTION: Retrieve a price by its ID. Optionally, related resources such as the variant can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/prices/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getPrice(
  priceId: number | string,
  params: GetPriceParams = {}
) {
  requiredCheck({ priceId });
  return $fetch<Price>({
    path: `/v1/prices/${priceId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Prices
DESCRIPTION: List all prices. Allows filtering by variant ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/prices/index.ts#L44-L48
LANGUAGE: ts
CODE:

export function listPrices(params: ListPricesParams = {}) {
  return $fetch<ListPrices>({
    path: `/v1/prices${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Product
DESCRIPTION: Retrieve a product by its ID. Optionally, related resources such as variants can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/products/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getProduct(
  productId: number | string,
  params: GetProductParams = {}
) {
  requiredCheck({ productId });
  return $fetch<Product>({
    path: `/v1/products/${productId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Products
DESCRIPTION: List all products. Allows filtering by store ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/products/index.ts#L44-L48
LANGUAGE: ts
CODE:

export function listProducts(params: ListProductsParams = {}) {
  return $fetch<ListProducts>({
    path: `/v1/products${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Store
DESCRIPTION: Retrieve a store by its ID. Optionally, related resources can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/stores/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getStore(
  storeId: number | string,
  params: GetStoreParams = {}
) {
  requiredCheck({ storeId });
  return $fetch<Store>({
    path: `/v1/stores/${storeId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Stores
DESCRIPTION: List all stores. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/stores/index.ts#L42-L46
LANGUAGE: ts
CODE:

export function listStores(params: ListStoresParams = {}) {
  return $fetch<ListStores>({
    path: `/v1/stores${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Subscription Invoice
DESCRIPTION: Retrieve a subscription invoice by its ID. Optionally, related resources such as store, subscription, or customer can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionInvoices/index.ts#L25-L33
LANGUAGE: ts
CODE:

export function getSubscriptionInvoice(
  subscriptionInvoiceId: number | string,
  params: GetSubscriptionInvoiceParams = {}
) {
  requiredCheck({ subscriptionInvoiceId });
  return $fetch<SubscriptionInvoice>({
    path: `/v1/subscription-invoices/${subscriptionInvoiceId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Subscription Invoices
DESCRIPTION: List all subscription invoices. Allows filtering by store ID, status, refunded status, or subscription ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionInvoices/index.ts#L50-L56
LANGUAGE: ts
CODE:

export function listSubscriptionInvoices(
  params: ListSubscriptionInvoicesParams = {}
) {
  return $fetch<ListSubscriptionInvoices>({
    path: `/v1/subscription-invoices${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Generate Subscription Invoice PDF
DESCRIPTION: Generate a PDF invoice for a specific subscription invoice. Requires subscription invoice ID and customer details. Optional notes and locale can be provided. Returns a link to download the invoice.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionInvoices/index.ts#L73-L88
LANGUAGE: ts
CODE:

export function generateSubscriptionInvoice(
  subscriptionInvoiceId: number | string,
  params: GenerateSubscriptionInvoiceParams
) {
  requiredCheck({ subscriptionInvoiceId });
  const searchParams = convertKeys(params);
  const queryString = new URLSearchParams(
    searchParams as Record<string, any>
  ).toString();
  const query = queryString ? `?${queryString}` : "";

  return $fetch<GenerateSubscriptionInvoice>({
    path: `/v1/subscription-invoices/${subscriptionInvoiceId}/generate-invoice${query}`,
    method: "POST",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Issue Subscription Invoice Refund
DESCRIPTION: Issues a partial refund for a subscription invoice. Requires the subscription invoice ID and the amount (in cents) to refund.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionInvoices/index.ts#L96-L115
LANGUAGE: ts
CODE:

export function issueSubscriptionInvoiceRefund(
  subscriptionInvoiceId: number | string,
  amount: number
) {
  requiredCheck({ subscriptionInvoiceId, amount });

  const attributes = { amount };

  return $fetch<SubscriptionInvoice>({
    path: `/v1/subscription-invoices/${subscriptionInvoiceId}/refund`,
    method: "POST",
    body: {
      data: {
        type: "subscription-invoices",
        id: subscriptionInvoiceId.toString(),
        attributes: convertKeys(attributes),
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Subscription Item
DESCRIPTION: Retrieve a subscription item by its ID. Optionally, related resources like subscription, price, or usage records can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionItems/index.ts#L26-L34
LANGUAGE: ts
CODE:

export function getSubscriptionItem(
  subscriptionItemId: number | string,
  params: GetSubscriptionItemParams = {}
) {
  requiredCheck({ subscriptionItemId });
  return $fetch<SubscriptionItem>({
    path: `/v1/subscription-items/${subscriptionItemId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Get Subscription Item Current Usage
DESCRIPTION: Retrieve a subscription item's current usage. Note: this endpoint is only for subscriptions with usage-based billing enabled.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionItems/index.ts#L44-L51
LANGUAGE: ts
CODE:

export function getSubscriptionItemCurrentUsage(
  subscriptionItemId: number | string
) {
  requiredCheck({ subscriptionItemId });
  return $fetch<SubscriptionItemCurrentUsage>({
    path: `/v1/subscription-items/${subscriptionItemId}/current-usage`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Subscription Items
DESCRIPTION: List all subscription items. Allows filtering by subscription ID or price ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionItems/index.ts#L65-L71
LANGUAGE: ts
CODE:

export function listSubscriptionItems(
  params: ListSubscriptionItemsParams = {}
) {
  return $fetch<ListSubscriptionItems>({
    path: `/v1/subscription-items${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Update a Subscription Item
DESCRIPTION: Update a subscription item, typically its quantity for quantity-based billing. Options for immediate invoicing or disabling prorations are available. This endpoint is not for usage-based billing.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptionItems/index.ts#L87-L111
LANGUAGE: ts
CODE:

export function updateSubscriptionItem(
  subscriptionItemId: string | number,
  updateSubscriptionItem: UpdateSubscriptionItem
): ReturnType<typeof _updateSubscriptionItem>;

export function updateSubscriptionItem(
  subscriptionItemId: string | number,
  updateSubscriptionItem: number | UpdateSubscriptionItem
) {
  return _updateSubscriptionItem(subscriptionItemId, updateSubscriptionItem);
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Subscription
DESCRIPTION: Retrieve a subscription by its ID. Optionally, related resources can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptions/index.ts#L24-L32
LANGUAGE: ts
CODE:

export function getSubscription(
  subscriptionId: number | string,
  params: GetSubscriptionParams = {}
) {
  requiredCheck({ subscriptionId });
  return $fetch<Subscription>({
    path: `/v1/subscriptions/${subscriptionId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Update a Subscription
DESCRIPTION: Update a subscription's details such as its variant, cancellation status, billing anchor, trial end date, or pause status.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptions/index.ts#L41-L77
LANGUAGE: ts
CODE:

export function updateSubscription(
  subscriptionId: string | number,
  updateSubscription: UpdateSubscription
) {
  requiredCheck({ subscriptionId });
  const {
    variantId,
    cancelled,
    billingAnchor,
    invoiceImmediately,
    disableProrations,
    pause,
    trialEndsAt,
  } = updateSubscription;

  const attributes = convertKeys({
    variantId,
    cancelled,
    billingAnchor,
    invoiceImmediately,
    disableProrations,
    pause,
    trialEndsAt,
  });

  return $fetch<Subscription>({
    path: `/v1/subscriptions/${subscriptionId}`,
    method: "PATCH",
    body: {
      data: {
        type: "subscriptions",
        id: subscriptionId.toString(),
        attributes,
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Cancel a Subscription
DESCRIPTION: Cancel a subscription by its ID. The subscription will enter a 'cancelled' state and will end based on its ends_at date.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptions/index.ts#L85-L92
LANGUAGE: ts
CODE:

export function cancelSubscription(subscriptionId: string | number) {
  requiredCheck({ subscriptionId });

  return $fetch<Subscription>({
    path: `/v1/subscriptions/${subscriptionId}`,
    method: "DELETE",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Subscriptions
DESCRIPTION: List all subscriptions. Allows filtering by store ID, order ID, order item ID, product ID, variant ID, user email, or status. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/subscriptions/index.ts#L111-L115
LANGUAGE: ts
CODE:

export function listSubscriptions(params: ListSubscriptionsParams = {}) {
  return $fetch<ListSubscriptions>({
    path: `/v1/subscriptions${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Usage Record
DESCRIPTION: Retrieve a usage record by its ID. Optionally, related resources such as the subscription item can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/usageRecords/index.ts#L23-L31
LANGUAGE: ts
CODE:

export function getUsageRecord(
  usageRecordId: number | string,
  params: GetUsageRecordParams = {}
) {
  requiredCheck({ usageRecordId });
  return $fetch<UsageRecord>({
    path: `/v1/usage-records/${usageRecordId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Usage Records
DESCRIPTION: List all usage records. Allows filtering by subscription item ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/usageRecords/index.ts#L45-L49
LANGUAGE: ts
CODE:

export function listUsageRecords(params: ListUsageRecordsParams = {}) {
  return $fetch<ListUsageRecords>({
    path: `/v1/usage-records${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Create a Usage Record
DESCRIPTION: Create a usage record for a subscription item. Requires quantity and subscription item ID. Optional action can be 'increment' (default) or 'set'.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/usageRecords/index.ts#L60-L84
LANGUAGE: ts
CODE:

export function createUsageRecord(usageRecord: NewUsageRecord) {
  const { quantity, action = "increment", subscriptionItemId } = usageRecord;
  requiredCheck({ quantity, subscriptionItemId });
  return $fetch<UsageRecord>({
    path: "/v1/usage-records",
    method: "POST",
    body: {
      data: {
        type: "usage-records",
        attributes: {
          quantity,
          action,
        },
        relationships: {
          "subscription-item": {
            data: {
              type: "subscription-items",
              id: subscriptionItemId.toString(),
            },
          },
        },
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Get Authenticated User
DESCRIPTION: Retrieve the authenticated user's details based on the provided API key.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/users/index.ts#L9-L13
LANGUAGE: ts
CODE:

export function getAuthenticatedUser() {
  return $fetch<User>({
    path: "/v1/users/me",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Variant
DESCRIPTION: Retrieve a product variant by its ID. Optionally, related resources such as product, files, or price model can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/variants/index.ts#L22-L30
LANGUAGE: ts
CODE:

export function getVariant(
  variantId: number | string,
  params: GetVariantParams = {}
) {
  requiredCheck({ variantId });
  return $fetch<Variant>({
    path: `/v1/variants/${variantId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Variants
DESCRIPTION: List all product variants. Allows filtering by product ID or status. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/variants/index.ts#L45-L49
LANGUAGE: ts
CODE:

export function listVariants(params: ListVariantsParams = {}) {
  return $fetch<ListVariants>({
    path: `/v1/variants${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Create a Webhook
DESCRIPTION: Create a webhook for a store. Requires store ID, URL for events, an array of event types, and a secret for signing requests.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/webhooks/index.ts#L24-L52
LANGUAGE: ts
CODE:

export function createWebhook(storeId: number | string, webhook: NewWebhook) {
  requiredCheck({ storeId });

  const { url, events, secret, testMode } = webhook;

  return $fetch<Webhook>({
    path: "/v1/webhooks",
    method: "POST",
    body: {
      data: {
        type: "webhooks",
        attributes: convertKeys({
          url,
          events,
          secret,
          testMode,
        }),
        relationships: {
          store: {
            data: {
              type: "stores",
              id: storeId.toString(),
            },
          },
        },
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Retrieve a Webhook
DESCRIPTION: Retrieve a webhook by its ID. Optionally, related resources such as the store can be included.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/webhooks/index.ts#L62-L70
LANGUAGE: ts
CODE:

export function getWebhook(
  webhookId: number | string,
  params: GetWebhookParams = {}
) {
  requiredCheck({ webhookId });
  return $fetch<Webhook>({
    path: `/v1/webhooks/${webhookId}${convertIncludeToQueryString(params.include)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Update a Webhook
DESCRIPTION: Update a webhook's details such as its URL, subscribed events, or secret.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/webhooks/index.ts#L79-L102
LANGUAGE: ts
CODE:

export function updateWebhook(
  webhookId: number | string,
  webhook: UpdateWebhook
) {
  requiredCheck({ webhookId });

  const { url, events, secret } = webhook;

  return $fetch<Webhook>({
    path: `/v1/webhooks/${webhookId}`,
    method: "PATCH",
    body: {
      data: {
        id: webhookId.toString(),
        type: "webhooks",
        attributes: convertKeys({
          url,
          events,
          secret,
        }),
      },
    },
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: Delete a Webhook
DESCRIPTION: Delete a webhook by its ID.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/webhooks/index.ts#L110-L116
LANGUAGE: ts
CODE:

export function deleteWebhook(webhookId: number | string) {
  requiredCheck({ webhookId });
  return $fetch<null>({
    path: `/v1/webhooks/${webhookId}`,
    method: "DELETE",
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END

TITLE: List Webhooks
DESCRIPTION: List all webhooks. Allows filtering by store ID. Pagination and inclusion of related resources are supported.
SOURCE: https://github.com/lmsqueezy/lemonsqueezy.js/blob/main/src/webhooks/index.ts#L130-L134
LANGUAGE: ts
CODE:

export function listWebhooks(params: ListWebhooksParams = {}) {
  return $fetch<ListWebhooks>({
    path: `/v1/webhooks${convertListParamsToQueryString(params)}`,
  });
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Ts
IGNORE_WHEN_COPYING_END