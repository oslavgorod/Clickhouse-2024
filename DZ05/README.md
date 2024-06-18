# ДЗ 05  
> CREATE TABLE tbl1  
(  
    UserID UInt64,  
    PageViews UInt8,  
    Duration UInt8,  
    Sign Int8,  
    Version UInt8  
)  
ENGINE = CollapsingMergeTree(Sign)  
ORDER BY UserID

