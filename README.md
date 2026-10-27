# AtScale Composite PostgreSQL Lab — Model A

This repository is the independently linkable AtScale SML project for the
sales side of the composite-model lab. It owns the canonical shared Date
dimension used by the other repositories.

## Branches

| Branch | AtScale warehouse ID | Purpose |
| --- | --- | --- |
| `main` | `pg_composite_control` | Package-ready positive control used by Model B and the Composite repository. |
| `isolated-service-account` | `pg_model_a` | Standalone Model A deployment using service account `svc_a`. |

SML does not interpolate values from an `.env` file. Create the warehouse in
AtScale with the exact ID required by the branch, or update the two
`as_connection` values before linking a fork.

## Objects owned here

- `lab_dim_date_control` — `shared.dim_date`
- `lab_fact_sales_control` — `src_a.fact_sales`
- `Lab Date Control` — canonical time dimension
- `m_sales_amount_sum_control` — sales amount metric
- `lab_sales_control` — component model consumed by the Composite repository

## Use in Design Center

1. Build the PostgreSQL fixture from the sibling
   [`atscale-composite-postgres-composite`](https://github.com/atscaleLV/atscale-composite-postgres-composite)
   repository.
2. In AtScale, create `pg_composite_control` for `main`, or `pg_model_a` for
   the isolated branch. Both point to database `atscale_lab`; the fixture
   supplies the corresponding database role.
3. Link this Git repository in Design Center and expose the desired branch.
4. Deploy the catalog. On `main`, downstream repos should pin this repo by a
   full commit SHA in `package.yml`.

`main` intentionally maps the shared and sales schemas through one AtScale
warehouse connection. This is the deployable control path for testing the
composite model and its aggregate. The isolated branch proves that Model A can
also run under its own service account; it is not the branch imported by the
cross-repository composite.

