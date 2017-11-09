### How to create a function

    
    def not_working(df, x):
        df = df[df.workingday == x] #np.where(bikes.workingday==x)
        return df.groupby('hour').total.mean().plot()

    not_working(bikes,0)
    
    The goal of this function is to plot the average of `total` depending if `workingday` is 0 or 1.
    
    1. Abstract the inputs into a function. df and x are can be anything, they are simply variables. 
    2. Assign df variable with condition, if necessary, in this case where column `workingday == x`. 
    3. return designates what actually shows up on your screen, or what gets returned. 
    4. When I actually call the function, I indicate my specifc dataframe (bikes), and the actual value I want x to be. 
     
