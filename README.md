# Morgan Mouland QAP 4
# Car Insurance Calculator

# Imports
import datetime

# Formats used
def FDollar2(DollarValue):
    # Function will accept a value and format it to $#,###.##.

    DollarValueStr = "${:,.2f}".format(DollarValue)

    return DollarValueStr

def FDateS(DateValue):
    # Function will accept a value and format it to yyyy-mm-dd.

    DateValueStr = DateValue.strftime("%Y-%m-%d")

    return DateValueStr

# Open the OSICDef.dat file and read the values
f = open("OSICDef.dat", "r")
POLICY_NUM = int(f.readline())
BASIC_PREMIUM = float(f.readline())
ADDITIONAL_CAR_DISCOUNT = float(f.readline())
COST_EXTRA_LIABILITY_COVERAGE = float(f.readline())
COST_GLASS_COVERAGE = float(f.readline())
COST_LOANER_CAR_COVERAGE = float(f.readline())
HST_RATE = float(f.readline())
PROCESSING_FEE = float(f.readline())
f.close()

# Inputs
while True:
    CustomerFirstName = input("Enter the customer first name (END to Quit): ").title()
    if CustomerFirstName.upper() == "END":
        break
    else:
        CustomerLastName = input("Enter the customer last name: ").title()
        Address = input("Enter the customer address: ").title()
        City = input("Enter the city: ").title()
        Prov = input("Enter the province: ").upper()
        PostalCode = input("Enter the postal code: ").upper()
        PhoneNumber = input("Enter the phone number: ")
        NumCars = int(input("Enter the number of cars: "))
        if NumCars >1:
            InsurancePremiumRateMultiple = BASIC_PREMIUM + (.25 * BASIC_PREMIUM) * NumCars
        else:
            InsurancePremiumRateMultiple = 0
        ExtraLiability = input("Would you like extra liability coverage? (up to 1,000,000) (Y/N): ").upper()
        if ExtraLiability == "Y":
            LiabilityCost = COST_EXTRA_LIABILITY_COVERAGE * NumCars
        elif ExtraLiability == "N":
            LiabilityCost = 0
        GlassCoverage = input("Would you like glass coverage? (Y/N): ").upper()
        if GlassCoverage == "Y":
            GlassCost = COST_GLASS_COVERAGE * NumCars
        elif GlassCoverage == "N":
            GlassCost = 0
        LoanerCar = input("Would you like a loaner car? (Y/N): ").upper()
        if LoanerCar == "Y":
            LonerCost = COST_LOANER_CAR_COVERAGE * NumCars
        elif LoanerCar == "N":
            LonerCost = 0

        # Calculations
        InsurancePremiumRateSingle = BASIC_PREMIUM
        TotalExtraCost = LiabilityCost + GlassCost + LonerCost
        TotalInsurancePremium = (InsurancePremiumRateSingle + InsurancePremiumRateMultiple) + TotalExtraCost
        HST = HST_RATE * TotalInsurancePremium
        TotalCost = TotalInsurancePremium + HST
        PolicyDate = str(FDateS(datetime.datetime.now()))

        #Payment plan option
        MonthlyPayment = input("Would you like to pay in full or monthly? (M/F): ").upper()
        if MonthlyPayment == "M":
            MonthlyCost = (TotalCost / 8) + PROCESSING_FEE
        elif MonthlyPayment == "F":
            MonthlyCost = 0

    # Outputs
        print()
        print(f"   One Stop Insurance Company")
        print()
        print("---------------------------------")
        print(f"Policy Number:       {POLICY_NUM} - {CustomerFirstName[0]}{CustomerLastName[0]} ")
        print(str(FDateS(datetime.datetime.now())))
        print("---------------------------------")
        print(f"Customer Name: {CustomerFirstName}, {CustomerLastName}")
        print("---------------------------------")
        print(f"Address:                {Address}")
        print(f"City:                   {City}")
        print(f"Province:               {Prov}")
        print(f"PostalCode:             {PostalCode}")
        print(f"Phone Number:           {PhoneNumber}")
        print(f"Number of Cars:         {NumCars}")
        print(f"Liability Insurance:    {FDollar2(LiabilityCost)}")
        print(f"Glass Coverage:         {FDollar2(GlassCost)}")
        print(f"Loaner Car:             {FDollar2(LonerCost)}")
        print(f"HST:                    {FDollar2(HST)}")
        print(f"SubTotal:               {FDollar2(TotalCost)}")
        if MonthlyPayment == "M":
            print(f"8 Monthly Payments of:  {FDollar2(MonthlyCost)}")

        print()
        print("---------------------------------")
        print()
        print("            Thank you")
        print()

        # Write results to file.
        f = open("Policies.dat", "a")
        f.write("{}\n".format(int(POLICY_NUM)))
        f.write("{}\n".format(str(PolicyDate)))
        f.write("{}\n".format(str(CustomerFirstName)))
        f.write("{}\n".format(str(CustomerLastName)))
        f.write("{}\n".format(str(Address)))
        f.write("{}\n".format(str(City)))
        f.write("{}\n".format(str(Prov)))
        f.write("{}\n".format(str(PostalCode)))
        f.write("{}\n".format(str(PhoneNumber)))
        f.write("{}\n".format(int(NumCars)))
        f.write("{}\n".format(float(LiabilityCost)))
        f.write("{}\n".format(float(GlassCost)))
        f.write("{}\n".format(float(LonerCost)))
        f.write("{}\n".format(float(MonthlyCost)))
        f.write("{}\n".format(float(TotalCost)))
        f.close()
        print()
        print("Policy information processed and saved.")
        print()
        POLICY_NUM += 1
